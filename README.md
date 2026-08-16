import json
import os
from datetime import datetime

TICKET_FILE = "tickets.json"

def load_tickets():
    if not os.path.exists(TICKET_FILE):
        return []
    with open(TICKET_FILE, "r", encoding="utf-8") as file:
        return json.load(file)

def save_tickets(tickets):
    with open(TICKET_FILE, "w", encoding="utf-8") as file:
        json.dump(tickets, file, indent=4)

def add_ticket(tickets):
    user = input("Uzytkownik: ")
    issue = input("Opis problemu: ")
    priority = input("Priorytet (Niski/Sredni/Wysoki): ")
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    ticket = {
        "id": len(tickets) + 1,
        "user": user,
        "issue": issue,
        "priority": priority,
        "status": "Otwarte",
        "date": timestamp
    }
    tickets.append(ticket)
    save_tickets(tickets)
    print(f"\nDodano zgloszenie ID: {ticket['id']}")

def view_tickets(tickets):
    if not tickets:
        print("\nBrak aktywnych zgloszen.")
        return
    print("\n--- LISTA ZGLOSZEN ---")
    for t in tickets:
        print(f"[{t['id']}] {t['user']} | Priorytet: {t['priority']} | Status: {t['status']}")
        print(f"Data: {t['date']}\nProblem: {t['issue']}\n{'-'*30}")

def resolve_ticket(tickets):
    try:
        ticket_id = int(input("Podaj ID zgloszenia do zamkniecia: "))
        for t in tickets:
            if t['id'] == ticket_id:
                t['status'] = "Zamkniete"
                save_tickets(tickets)
                print(f"\nZgloszenie {ticket_id} zostalo zamkniete.")
                return
        print("\nNie znaleziono zgloszenia o podanym ID.")
    except ValueError:
        print("\nNieprawidlowe ID.")

def main():
    tickets = load_tickets()
    while True:
        print("\n=== SYSTEM ZARZADZANIA ZGLOSZENIAMI ===")
        print("1. Dodaj zgloszenie")
        print("2. Wyswietl zgloszenia")
        print("3. Zamknij zgloszenie")
        print("4. Wyjscie")
        
        choice = input("Wybierz opcje (1-4): ")
        
        if choice == '1':
            add_ticket(tickets)
        elif choice == '2':
            view_tickets(tickets)
        elif choice == '3':
            resolve_ticket(tickets)
        elif choice == '4':
            print("Zamykanie programu...")
            break
        else:
            print("Nieznana opcja, sprobuj ponownie.")

if __name__ == "__main__":
    main()
