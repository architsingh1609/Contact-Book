# Contact-Book
This is a simple Contact Book application developed in Python that allows users to manage their contacts. The program supports functionalities such as adding, viewing, searching, updating, and deleting contacts, with data stored in a JSON file for persistence.
# Developed by Archit Singh
# This software is in the public domain and can be freely used, modified, and distributed.

import json

class ContactBook:
    def __init__(self, filename="contacts.json"):
        self.filename = filename
        self.contacts = self.load_contacts()

    def load_contacts(self):
        try:
            with open(self.filename, "r") as file:
                return json.load(file)
        except (FileNotFoundError, json.JSONDecodeError):
            return {}

    def save_contacts(self):
        with open(self.filename, "w") as file:
            json.dump(self.contacts, file, indent=4)

    def add_contact(self, name, phone, email):
        self.contacts[name] = {"phone": phone, "email": email}
        self.save_contacts()
        print(f"Contact {name} added successfully.")

    def view_contacts(self):
        if not self.contacts:
            print("No contacts available.")
            return
        for name, details in self.contacts.items():
            print(f"Name: {name}, Phone: {details['phone']}, Email: {details['email']}")

    def search_contact(self, name):
        if name in self.contacts:
            details = self.contacts[name]
            print(f"Name: {name}, Phone: {details['phone']}, Email: {details['email']}")
        else:
            print("Contact not found.")

    def update_contact(self, name, phone=None, email=None):
        if name in self.contacts:
            if phone:
                self.contacts[name]['phone'] = phone
            if email:
                self.contacts[name]['email'] = email
            self.save_contacts()
            print(f"Contact {name} updated successfully.")
        else:
            print("Contact not found.")

    def delete_contact(self, name):
        if name in self.contacts:
            del self.contacts[name]
            self.save_contacts()
            print(f"Contact {name} deleted successfully.")
        else:
            print("Contact not found.")


def main():
    print("Developed by Archit Singh\n")  # Display developer name at the top
    book = ContactBook()
    while True:
        print("\nContact Book Menu:")
        print("1. Add Contact")
        print("2. View Contacts")
        print("3. Search Contact")
        print("4. Update Contact")
        print("5. Delete Contact")
        print("6. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            name = input("Enter name: ")
            phone = input("Enter phone: ")
            email = input("Enter email: ")
            book.add_contact(name, phone, email)
        elif choice == "2":
            book.view_contacts()
        elif choice == "3":
            name = input("Enter name to search: ")
            book.search_contact(name)
        elif choice == "4":
            name = input("Enter name to update: ")
            phone = input("Enter new phone (leave blank to keep unchanged): ") or None
            email = input("Enter new email (leave blank to keep unchanged): ") or None
            book.update_contact(name, phone, email)
        elif choice == "5":
            name = input("Enter name to delete: ")
            book.delete_contact(name)
        elif choice == "6":
            print("Exiting Contact Book. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
