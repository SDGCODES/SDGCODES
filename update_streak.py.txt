import os
import re

def get_day_folders():
    day_pattern = re.compile(r"Day(\d+)", re.IGNORECASE)
    days = []
    for folder in os.listdir():
        if os.path.isdir(folder):
            match = day_pattern.fullmatch(folder)
            if match:
                days.append(int(match.group(1)))
    return sorted(days)

def count_java_files(day_folders):
    total = 0
    for day in day_folders:
        path = f"Day{day:02d}"
        if os.path.exists(path):
            for file in os.listdir(path):
                if file.endswith(".java"):
                    total += 1
    return total

def update_readme(streak, total_problems):
    with open("README.md", "r", encoding="utf-8") as f:
        lines = f.readlines()

    with open("README.md", "w", encoding="utf-8") as f:
        for line in lines:
            if "Current Streak" in line:
                f.write(f"- **Current Streak:** {streak} Days\n")
            elif "Total Problems Solved" in line:
                f.write(f"- **Total Problems Solved:** {total_problems}\n")
            else:
                f.write(line)

if __name__ == "__main__":
    day_folders = get_day_folders()
    streak = len(day_folders)
    total_problems = count_java_files(day_folders)
    update_readme(streak, total_problems)
    print(f"✅ README updated: {streak} days, {total_problems} problems solved.")
