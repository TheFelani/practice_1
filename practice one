import subprocess
import time

processes = []

number = int(input("Number of programs: "))

for i in range(number):
    path = input(f"Program {i + 1} path: ")
    burst = float(input(f"Estimated burst time for P{i + 1} (seconds): "))

    if burst <= 0:
        raise ValueError("Burst time must be positive")

    processes.append({
        "name": f"P{i + 1}",
        "path": path,
        "burst": burst,
        "arrival": time.perf_counter(),
        "start": None,
        "finish": None,
        "waiting": 0,
        "response_ratio": 0,
        "runtime": 0,
        "return_code": None
    })

current_time = time.perf_counter()

for p in processes:
    p["arrival"] = current_time

completed = []

while processes:
    current_time = time.perf_counter()

    for p in processes:
        p["waiting"] = current_time - p["arrival"]
        p["response_ratio"] = (
            p["waiting"] + p["burst"]
        ) / p["burst"]

    selected = max(
        processes,
        key=lambda p: p["response_ratio"]
    )

    print(
        f'\nRunning {selected["name"]} '
        f'(Response Ratio: {selected["response_ratio"]:.2f})'
    )

    selected["start"] = time.perf_counter()

    try:
        result = subprocess.run([selected["path"]], check=False)
        selected["return_code"] = result.returncode
    except OSError as error:
        print("Error:", error)
        selected["return_code"] = -1

    selected["finish"] = time.perf_counter()
    selected["runtime"] = (
        selected["finish"] - selected["start"]
    )

    completed.append(selected)
    processes.remove(selected)

print("\nResults")
print("Program\tWaiting\tResponse Ratio\tRuntime\tReturn Code")

for p in completed:
    print(
        f'{p["name"]}\t'
        f'{p["waiting"]:.2f}\t'
        f'{p["response_ratio"]:.2f}\t\t'
        f'{p["runtime"]:.2f}\t'
        f'{p["return_code"]}'
    )
