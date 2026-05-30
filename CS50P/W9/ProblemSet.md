# Final Project
```python
import sys
import urllib.request
import urllib.error
import json
import socket

def check_argument():
    """Validate command-line arguments and return the target URL."""
    if len(sys.argv) != 2:
        sys.exit("Usage: python apirecon.py [-u] [--url] https://example.com")
    
    url = sys.argv[1]
    
    # Check if user passed a flag instead of URL
    if url in ["-u", "--url"]:
        sys.exit("Usage: python apirecon.py [-u] [--url] https://example.com")
    
    # Basic URL validation
    if not url.startswith("http://") and not url.startswith("https://"):
        sys.exit("Error: URL must start with http:// or https://")
    
    return url

def rest_scan(url):
    """Scan for REST API endpoints."""
    print("\n[+] Scanning for REST API endpoints...")
    
    rest_endpoints = [
        "/api", "/api/v1", "/api/v2", "/v1", "/v2",
        "/users", "/login", "/auth", "/token",
        "/health", "/status", "/ping"
    ]
    
    # Check common REST endpoints
    found = []
    for endpoint in rest_endpoints[:3]:  # Test first few to avoid too many requests
        test_url = url.rstrip("/") + endpoint
        try:
            req = urllib.request.Request(test_url, method="GET")
            req.add_header("User-Agent", "Mozilla/5.0")
            response = urllib.request.urlopen(req, timeout=5)
            if response.getcode() == 200:
                found.append(test_url)
                print(f"    [FOUND] {test_url} (HTTP {response.getcode()})")
        except urllib.error.HTTPError as e:
            if e.code < 400:
                found.append(test_url)
                print(f"    [FOUND] {test_url} (HTTP {e.code})")
        except (urllib.error.URLError, socket.timeout, Exception) as e:
            pass  # Endpoint not found or unreachable
    
    if not found:
        print("    [-] No common REST endpoints found")
    
    return found

def gql_scan(url):
    """Scan for GraphQL endpoints."""
    print("\n[+] Scanning for GraphQL endpoints...")
    
    gql_endpoints = ["/graphql", "/graph", "/api/graphql", "/v1/graphql"]
    
    found = []
    for endpoint in gql_endpoints:
        test_url = url.rstrip("/") + endpoint
        
        # Try GraphQL introspection query
        introspection_query = '{"query": "{__schema{types{name}}}"}'
        try:
            req = urllib.request.Request(test_url, data=introspection_query.encode(), method="POST")
            req.add_header("Content-Type", "application/json")
            req.add_header("User-Agent", "Mozilla/5.0")
            response = urllib.request.urlopen(req, timeout=5)
            
            if response.getcode() == 200:
                try:
                    data = json.loads(response.read().decode())
                    if "data" in data or "schema" in str(data).lower():
                        found.append(test_url)
                        print(f"    [FOUND] {test_url} (GraphQL endpoint detected)")
                except json.JSONDecodeError:
                    found.append(test_url)
                    print(f"    [FOUND] {test_url} (possible GraphQL)")
        except urllib.error.HTTPError as e:
            if e.code == 400 or e.code == 401:  # GraphQL often returns 400 for bad queries
                found.append(test_url)
                print(f"    [FOUND] {test_url} (HTTP {e.code}, likely GraphQL)")
        except (urllib.error.URLError, socket.timeout, Exception) as e:
            pass
    
    if not found:
        print("    [-] No GraphQL endpoints found")
    
    return found

def websocket_scan(url):
    """Check for WebSocket upgrade capability."""
    print("\n[+] Checking for WebSocket support...")
    
    try:
        req = urllib.request.Request(url, method="GET")
        req.add_header("Upgrade", "websocket")
        req.add_header("Connection", "Upgrade")
        req.add_header("Sec-WebSocket-Key", "dGhlIHNhbXBsZSBub25jZQ==")
        req.add_header("Sec-WebSocket-Version", "13")
        req.add_header("User-Agent", "Mozilla/5.0")
        
        response = urllib.request.urlopen(req, timeout=5)
        code = response.getcode()
        
        if code == 101:  # Switching Protocols
            print(f"    [FOUND] {url} supports WebSocket upgrade (HTTP 101)")
            return [url]
        else:
            print(f"    [-] WebSocket upgrade not supported (HTTP {code})")
            
    except urllib.error.HTTPError as e:
        if e.code == 101:
            print(f"    [FOUND] {url} supports WebSocket upgrade (HTTP 101)")
            return [url]
        elif e.code == 400 or e.code == 404:
            print(f"    [-] WebSocket not supported at {url} (HTTP {e.code})")
        else:
            print(f"    [-] WebSocket check failed (HTTP {e.code})")
    except (urllib.error.URLError, socket.timeout, Exception) as e:
        print(f"    [-] Could not check WebSocket support: {type(e).__name__}")
    
    return []

def soap_scan(url):
    """Scan for SOAP API endpoints."""
    print("\n[+] Scanning for SOAP API endpoints...")
    
    soap_endpoints = [
        "/service", "/services", "/soap", "/api/soap",
        "/ws", "/wsdl", "/soap/v1", "/services/Service"
    ]
    
    found = []
    for endpoint in soap_endpoints[:3]:
        test_url = url.rstrip("/") + endpoint
        
        try:
            req = urllib.request.Request(test_url, method="GET")
            req.add_header("User-Agent", "Mozilla/5.0")
            response = urllib.request.urlopen(req, timeout=5)
            
            if response.getcode() == 200:
                content = response.read().decode("utf-8", errors="ignore")
                if "soap" in content.lower() or "wsdl" in content.lower() or "envelope" in content.lower():
                    found.append(test_url)
                    print(f"    [FOUND] {test_url} (SOAP/WSDL detected)")
                else:
                    print(f"    [INFO] {test_url} (HTTP {response.getcode()}, checking content...)")
        except urllib.error.HTTPError as e:
            if e.code == 500:
                found.append(test_url)
                print(f"    [FOUND] {test_url} (HTTP {e.code}, likely SOAP)")
        except (urllib.error.URLError, socket.timeout, Exception) as e:
            pass
    
    if not found:
        print("    [-] No SOAP endpoints found")
    
    return found

def main():
    """Main function to run all API reconnaissance scans."""
    print("=" * 60)
    print("           API Reconnaissance Tool")
    print("=" * 60)
    
    target = check_argument()
    print(f"Target: {target}")
    
    results = {
        "REST": rest_scan(target),
        "GraphQL": gql_scan(target),
        "WebSocket": websocket_scan(target),
        "SOAP": soap_scan(target)
    }
    
    print("\n" + "=" * 60)
    print("           Scan Summary")
    print("=" * 60)
    
    total_found = sum(len(v) for v in results.values())
    for scan_type, endpoints in results.items():
        status = f"Found {len(endpoints)}" if endpoints else "None found"
        print(f"  {scan_type:15} | {status}")
    
    print(f"\n  Total endpoints found: {total_found}")
    print("=" * 60)

if __name__ == "__main__":
    main()
```


# Test
```python
import sys
import io
from unittest.mock import patch
import urllib.error
import socket
import apirecon


def test_check_argument_valid_http():
    with patch.object(sys, 'argv', ['apirecon.py', 'http://example.com']):
        result = apirecon.check_argument()
        assert result == 'http://example.com'


def test_check_argument_valid_https():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        result = apirecon.check_argument()
        assert result == 'https://example.com'


def test_check_argument_missing_argument():
    with patch.object(sys, 'argv', ['apirecon.py']):
        try:
            apirecon.check_argument()
            assert False, "Should have raised SystemExit"
        except SystemExit as e:
            assert "Usage" in str(e)


def test_check_argument_flag_instead_of_url():
    with patch.object(sys, 'argv', ['apirecon.py', '-u']):
        try:
            apirecon.check_argument()
            assert False, "Should have raised SystemExit"
        except SystemExit as e:
            assert "Usage" in str(e)


def test_check_argument_invalid_scheme():
    with patch.object(sys, 'argv', ['apirecon.py', 'ftp://example.com']):
        try:
            apirecon.check_argument()
            assert False, "Should have raised SystemExit"
        except SystemExit as e:
            assert "Error" in str(e)


def test_rest_scan_returns_list():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        result = apirecon.rest_scan(url)
        assert isinstance(result, list)


def test_rest_scan_prints_header():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            apirecon.rest_scan(url)
            output = fake_out.getvalue()
            assert "Scanning for REST API endpoints" in output


def test_gql_scan_returns_list():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        result = apirecon.gql_scan(url)
        assert isinstance(result, list)


def test_gql_scan_prints_header():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            apirecon.gql_scan(url)
            output = fake_out.getvalue()
            assert "Scanning for GraphQL endpoints" in output


def test_websocket_scan_returns_list():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        result = apirecon.websocket_scan(url)
        assert isinstance(result, list)


def test_websocket_scan_prints_header():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            apirecon.websocket_scan(url)
            output = fake_out.getvalue()
            assert "Checking for WebSocket support" in output


def test_soap_scan_returns_list():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        result = apirecon.soap_scan(url)
        assert isinstance(result, list)


def test_soap_scan_prints_header():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        url = apirecon.check_argument()
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            apirecon.soap_scan(url)
            output = fake_out.getvalue()
            assert "Scanning for SOAP API endpoints" in output


def test_main_output_contains_title():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            try:
                apirecon.main()
                output = fake_out.getvalue()
                assert "API Reconnaissance Tool" in output
            except (urllib.error.URLError, socket.timeout):
                pass


def test_main_output_contains_summary():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com']):
        with patch('sys.stdout', new=io.StringIO()) as fake_out:
            try:
                apirecon.main()
                output = fake_out.getvalue()
                assert "Scan Summary" in output
            except (urllib.error.URLError, socket.timeout):
                pass


def test_url_with_trailing_slash():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com/']):
        result = apirecon.check_argument()
        assert result == 'https://example.com/'


def test_multiple_arguments():
    with patch.object(sys, 'argv', ['apirecon.py', 'https://example.com', 'extra']):
        try:
            apirecon.check_argument()
            assert False, "Should have raised SystemExit"
        except SystemExit as e:
            assert "Usage" in str(e)


def test_empty_string_argument():
    with patch.object(sys, 'argv', ['apirecon.py', '']):
        try:
            apirecon.check_argument()
            assert False, "Should have raised SystemExit"
        except SystemExit as e:
            assert "Error" in str(e)
```
