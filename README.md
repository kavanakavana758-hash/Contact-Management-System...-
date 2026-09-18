# Contact-Management-System...-
#include <iostream>
#include <vector>
#include <fstream>
#include <string>
#include <algorithm>
#include <limits>

using namespace std;

// Contact structure
struct Contact {
    string name;
    string phone;
    string email;
    string address;
};

// File name for permanent storage
const string FILE_NAME = "contacts.txt";

// Function to save contacts to file
void saveContacts(const vector<Contact>& contacts) {
    ofstream file(FILE_NAME);

    if (!file) {
        cout << "Error: Unable to open file for saving.\n";
        return;
    }

    for (const Contact& c : contacts) {
        file << c.name << '\n';
        file << c.phone << '\n';
        file << c.email << '\n';
        file << c.address << '\n';
        file << "--------------------------------\n";
    }

    file.close();
}

// Function to load contacts from file
void loadContacts(vector<Contact>& contacts) {
    ifstream file(FILE_NAME);

    if (!file) {
        return; // File does not exist yet
    }

    Contact c;
    string separator;

    while (getline(file, c.name)) {
        if (!getline(file, c.phone))
            break;

        if (!getline(file, c.email))
            break;

        if (!getline(file, c.address))
            break;

        getline(file, separator);

        contacts.push_back(c);
    }

    file.close();
}

// Function to add a contact
void addContact(vector<Contact>& contacts) {
    Contact c;

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "\nEnter Name: ";
    getline(cin, c.name);

    cout << "Enter Phone: ";
    getline(cin, c.phone);

    cout << "Enter Email: ";
    getline(cin, c.email);

    cout << "Enter Address: ";
    getline(cin, c.address);

    contacts.push_back(c);

    cout << "\nContact added successfully!\n";
}

// Function to display all contacts
void displayContacts(const vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\nNo contacts available.\n";
        return;
    }

    cout << "\n========== CONTACT LIST ==========\n";

    for (size_t i = 0; i < contacts.size(); i++) {
        cout << "\nContact " << i + 1 << endl;
        cout << "Name    : " << contacts[i].name << endl;
        cout << "Phone   : " << contacts[i].phone << endl;
        cout << "Email   : " << contacts[i].email << endl;
        cout << "Address : " << contacts[i].address << endl;
        cout << "---------------------------------\n";
    }
}

// Function to search contacts
void searchContact(const vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\nNo contacts available.\n";
        return;
    }

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string search;
    cout << "\nEnter name or phone to search: ";
    getline(cin, search);

    bool found = false;

    for (const Contact& c : contacts) {
        // Search by name or phone
        if (c.name.find(search) != string::npos ||
            c.phone.find(search) != string::npos) {

            cout << "\nContact Found!\n";
            cout << "Name    : " << c.name << endl;
            cout << "Phone   : " << c.phone << endl;
            cout << "Email   : " << c.email << endl;
            cout << "Address : " << c.address << endl;
            cout << "---------------------------------\n";

            found = true;
        }
    }

    if (!found) {
        cout << "\nNo matching contact found.\n";
    }
}

// Function to edit a contact
void editContact(vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\nNo contacts available.\n";
        return;
    }

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string search;
    cout << "\nEnter name or phone of contact to edit: ";
    getline(cin, search);

    for (size_t i = 0; i < contacts.size(); i++) {
        if (contacts[i].name.find(search) != string::npos ||
            contacts[i].phone.find(search) != string::npos) {

            cout << "\nContact found.\n";
            cout << "Name    : " << contacts[i].name << endl;
            cout << "Phone   : " << contacts[i].phone << endl;
            cout << "Email   : " << contacts[i].email << endl;
            cout << "Address : " << contacts[i].address << endl;

            cout << "\nEnter new details:\n";

            cout << "New Name: ";
            getline(cin, contacts[i].name);

            cout << "New Phone: ";
            getline(cin, contacts[i].phone);

            cout << "New Email: ";
            getline(cin, contacts[i].email);

            cout << "New Address: ";
            getline(cin, contacts[i].address);

            cout << "\nContact updated successfully!\n";
            return;
        }
    }

    cout << "\nContact not found.\n";
}

// Function to delete a contact
void deleteContact(vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\nNo contacts available.\n";
        return;
    }

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string search;
    cout << "\nEnter name or phone of contact to delete: ";
    getline(cin, search);

    for (auto it = contacts.begin(); it != contacts.end(); ++it) {
        if (it->name.find(search) != string::npos ||
            it->phone.find(search) != string::npos) {

            cout << "\nContact found:\n";
            cout << "Name  : " << it->name << endl;
            cout << "Phone : " << it->phone << endl;

            char choice;
            cout << "\nAre you sure you want to delete this contact? (Y/N): ";
            cin >> choice;

            if (choice == 'Y' || choice == 'y') {
                contacts.erase(it);
                cout << "\nContact deleted successfully!\n";
            } else {
                cout << "\nDelete operation cancelled.\n";
            }

            return;
        }
    }

    cout << "\nContact not found.\n";
}

// Main function
int main() {
    vector<Contact> contacts;

    // Load contacts when program starts
    loadContacts(contacts);

    int choice;

    do {
        cout << "\n\n";
        cout << "====================================\n";
        cout << "       CONTACT MANAGEMENT SYSTEM    \n";
        cout << "====================================\n";
        cout << "1. Add Contact\n";
        cout << "2. Display All Contacts\n";
        cout << "3. Search Contact\n";
        cout << "4. Edit Contact\n";
        cout << "5. Delete Contact\n";
        cout << "6. Save Contacts\n";
        cout << "7. Exit\n";
        cout << "====================================\n";

        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {

            case 1:
                addContact(contacts);
                break;

            case 2:
                displayContacts(contacts);
                break;

            case 3:
                searchContact(contacts);
                break;

            case 4:
                editContact(contacts);
                break;

            case 5:
                deleteContact(contacts);
                break;

            case 6:
                saveContacts(contacts);
                cout << "\nContacts saved successfully!\n";
                break;

            case 7:
                // Save automatically before exiting
                saveContacts(contacts);
                cout << "\nContacts saved successfully.\n";
                cout << "Thank you for using Contact Management System!\n";
                break;

            default:
                cout << "\nInvalid choice! Please try again.\n";
        }

    } while (choice != 7);

    return 0;
}