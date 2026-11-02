import json
from pathlib import Path

FILE = Path("water.json")
GOAL = 2000

def load_data():
    if FILE.exists():
        return json.loads(FILE.read_text())
    return {"intake": 0}

def save_data(data):
    FILE.write_text(json.dumps(data, indent=4))

def main():
    data = load_data()

    while True:
        print("\n1. Add water")
        print("2. Show progress")
        print("3. Reset")
        print("4. Exit")

        choice = input("Choose: ")

        if choice == "1":
            try:
                amount = int(input("Amount in ml: "))
                if amount <= 0:
                    print("Enter a positive amount.")
                    continue

                data["intake"] += amount
                save_data(data)
                print("Water added!")

            except ValueError:
                print("Invalid amount.")

        elif choice == "2":
            intake = data["intake"]
            remaining = max(0, GOAL - intake)

            print(f"Consumed: {intake} ml")
            print(f"Remaining: {remaining} ml")
            print(f"Progress: {min(100, intake / GOAL * 100):.1f}%")

        elif choice == "3":
            data["intake"] = 0
            save_data(data)
            print("Progress reset.")

        elif choice == "4":
            break

if __name__ == "__main__":
    main()
