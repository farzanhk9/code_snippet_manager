import json
import os

FILENAME = "snippets.json"

# ---------- Data Handling ----------
def load_snippets():
    if os.path.exists(FILENAME):
        with open(FILENAME, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_snippets(snippets):
    with open(FILENAME, "w", encoding="utf-8") as f:
        json.dump(snippets, f, indent=4, ensure_ascii=False)

# ---------- Core Functions ----------
def add_snippet(snippets):
    print("\n➕ Add a New Code Snippet")
    title = input("🧠 Title: ").strip()
    language = input("💻 Language (e.g. Python, JS, HTML): ").strip().capitalize()
    description = input("📝 Description: ").strip()
    code = []
    print("💬 Enter your code (type 'END' to finish):")
    while True:
        line = input()
        if line.strip().upper() == "END":
            break
        code.append(line)
    snippet = {
        "title": title,
        "language": language,
        "description": description,
        "code": "\n".join(code)
    }
    snippets.append(snippet)
    save_snippets(snippets)
    print("✅ Snippet saved successfully!")

def view_snippets(snippets):
    if not snippets:
        print("📭 No snippets saved yet.")
        return
    print("\n=== 💾 Your Code Snippets ===")
    for i, s in enumerate(snippets, 1):
        print(f"{i}. [{s['language']}] {s['title']} — {s['description']}")

def search_snippets(snippets):
    keyword = input("🔍 Search by keyword or language: ").strip().lower()
    results = [s for s in snippets if keyword in s["title"].lower() or keyword in s["language"].lower() or keyword in s["description"].lower()]
    
    if not results:
        print("❌ No matching snippets found.")
        return
    
    print("\n=== 🔎 Search Results ===")
    for s in results:
        print(f"\n📘 {s['title']} ({s['language']})")
        print(f"📝 {s['description']}")
        print(f"💻 Code:\n{s['code']}")
        print("-" * 40)

# ---------- Main Menu ----------
def main():
    snippets = load_snippets()
    while True:
        print("\n=== CODE SNIPPET MANAGER ===")
        print("1️⃣ Add new snippet")
        print("2️⃣ View all snippets")
        print("3️⃣ Search snippets")
        print("4️⃣ Exit")

        choice = input("👉 Choose an option: ").strip()
        if choice == "1":
            add_snippet(snippets)
        elif choice == "2":
            view_snippets(snippets)
        elif choice == "3":
            search_snippets(snippets)
        elif choice == "4":
            print("👋 Goodbye! Keep coding smart!")
            break
        else:
            print("⚠️ Invalid choice.")

if __name__ == "__main__":
    main()
