import socket
from concurrent.futures import ThreadPoolExecutor

# Function to scan a single port
def scan_port(target, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(0.5)

        result = sock.connect_ex((target, port))
        if result == 0:
            print(f"[+] Port {port} is OPEN")

        sock.close()

    except Exception:
        pass


def main():
    target = input("Enter target IP or hostname: ")

    try:
        target_ip = socket.gethostbyname(target)
    except socket.gaierror:
        print("Invalid hostname.")
        return

    print(f"\nScanning {target} ({target_ip})...\n")

    start_port = int(input("Start Port: "))
    end_port = int(input("End Port: "))

    with ThreadPoolExecutor(max_workers=100) as executor:
        for port in range(start_port, end_port + 1):
            executor.submit(scan_port, target_ip, port)

    print("\nScan Complete.")


if __name__ == "__main__":
    main()
