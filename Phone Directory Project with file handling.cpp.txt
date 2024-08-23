
#include <iostream>
#include <fstream>
#include <cstring>
#include <ctime>

using namespace std;

class dnode
{
public:
    char number[50];
    char gmail[40];
    char name[30];
    time_t timestamp; // New member to store the timestamp
    dnode *prev, *next;

    dnode(char n[], char r[], char g[])
    {
        strcpy(name, n);
        strcpy(number, r);
        strcpy(gmail, g);
        timestamp = std::time(0);
        next = NULL;
        prev = NULL;
    }
    friend class dlist;
};

class dlist
{
    dnode *head, *temp, *ptr;

    dnode *ptr1, *ptr2, *dup;
    int modificationCount; // Counter to keep track of the number of modifications for each contact

public:
    dnode *prevn;

    void insert();
    void sort();
    void deletecontact(char n[20]);
    void deletesamenumber();
    void deletesamename();
    void deletesamegmail();
    void searchbyname(char p[20]);
    void searchbynumber(char no[30]);
    void searchbygmail(char g[20]);
    void updateHistory(dnode *contact, const char *field, const char *oldValue);
    void displayContactHistory(char n[20]);
    const char *getFieldName(int field);

    void accept();
    void display();
    void update(char ch[10]);
    void saveToFile(const char *filename);
    void loadFromFile(const char *filename);

    dlist()
    {
        head = NULL;
        temp = NULL;
        ptr = NULL;
        ptr1 = NULL;
        ptr2 = NULL;
        dup = NULL;
        modificationCount = 0;
    }
};

void dlist::updateHistory(dnode *contact, const char *field, const char *oldValue)
{
    // Function to update contact history
    // For simplicity, I'll just print the history here. In a real-world scenario, you may want to store it in a data structure.
    cout << "\nModification History for Contact '" << contact->name << "':\n";
    cout << "Modification " << ++modificationCount << " at " << ctime(&contact->timestamp);
    cout << "Field: " << field << "\n";
    cout << "Old Value: " << oldValue << "\n\n";
}

void dlist::displayContactHistory(char n[20])
{
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(n, ptr->name) == 0)
        {
            // Display contact history
            // For simplicity, I'll just print the history here. In a real-world scenario, you may want to display it more elegantly.
            cout << "\nContact History for '" << ptr->name << "':\n";
            cout << "Total Modifications: " << modificationCount << "\n\n";
            return;
        }
        ptr = ptr->next;
    }

    cout << "Contact not found.\n";
}

const char *dlist::getFieldName(int field)
{
    // Function to return the field name based on the field code
    switch (field)
    {
    case 1:
        return "NAME";
    case 2:
        return "PHONE NUMBER";
    case 3:
        return "G-MAIL";
    default:
        return "";
    }
}

void dlist::accept()
{
    char number[50];
    char gmail[40];
    char name[30];
    char ans;
    do
    {
        cout << "ENTER NAME:";
        cin >> name;
        cout << "ENTER NUMBER:";
        cin >> number;
        while (strlen(number) != 10)
        {
            cout << "ENTER VALID NUMBER:";
            cin >> number;
        }
        cout << "ENTER G-MAIL:";
        cin >> gmail;
        temp = new dnode(name, number, gmail);
        if (head == NULL)
        {
            head = temp;
        }
        else
        {
            ptr = head;
            while (ptr->next != NULL)
            {
                ptr = ptr->next;
            }
            ptr->next = temp;
            temp->prev = ptr;
        }
        cout << "DO YOU WANT TO CONTINUE?";
        cin >> ans;
    } while (ans == 'y');
}

void dlist::display()
{
    ptr = head; //start the node
    while (ptr != NULL) //traverse till last
    {
        cout << "\n\nNAME  ::\t" << ptr->name;
        cout << "\nNUMBER::\t+91-" << ptr->number;
        cout << "\nG-MAIL::\t" << ptr->gmail;
        ptr = ptr

->next;
    }
}

void dlist::insert()
{
    accept();
}

void dlist::sort()
{
    dnode *i, *j;
    int temp;
    char n[10];
    for (i = head; i->next != NULL; i = i->next)
    {
        for (j = i->next; j != NULL; j = j->next)
        {
            temp = strcmp(i->name, j->name);
            if (temp > 0)
            {
                strcpy(n, i->name);
                strcpy(i->name, j->name);
                strcpy(j->name, n);
            }
        }
    }
}

void dlist::deletecontact(char s[20])
{
    int c = 0;
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(s, ptr->name) == 0)
        {
            c = 1;
            break;
        }
        else
        {
            c = 2;
        }
        ptr = ptr->next;
    }
    if (c == 1 && ptr != head && ptr->next != NULL)
    {
        ptr->prev->next = ptr->next;
        ptr->next->prev = ptr->prev;
        delete (ptr);
        cout << "YOUR CONTACT IS SUCCESSFULLY DELETED\n\n";
    }
    if (ptr == head)
    {
        head = head->next;
        head->prev = NULL;
        delete (ptr);
        cout << "YOUR CONTACT IS SUCCESSFULLY DELETED\n\n";
    }
    if (ptr->next == NULL)
    {
        ptr->prev->next = NULL;
        ptr->prev = NULL;
        delete (ptr);
        cout << "YOUR CONTACT IS SUCCESSFULLY DELETED\n\n";
    }
    if (c == 2)
    {
        cout << "YOUR ENTERED NAME IS NOT IN THE LIST...";
    }
}

void dlist::deletesamename()
{
    ptr1 = head;
    while (ptr1 != NULL && ptr1->next != NULL)
    {
        ptr2 = ptr1;
        while (ptr2->next != NULL)
        {
            if (strcmp(ptr1->name, ptr2->next->name) == 0)
            {
                dup = ptr2->next;
                ptr2->next = ptr2->next->next;
                delete (dup);
            }
            else
            {
                ptr2 = ptr2->next;
            }
        }
        ptr1 = ptr1->next;
    }
}

void dlist::deletesamegmail()
{
    ptr1 = head;
    while (ptr1 != NULL && ptr1->next != NULL)
    {
        ptr2 = ptr1;
        while (ptr2->next != NULL)
        {
            if (strcmp(ptr1->gmail, ptr2->next->gmail) == 0)
            {
                dup = ptr2->next;
                ptr2->next = ptr2->next->next;
                delete (dup);
            }
            else
            {
                ptr2 = ptr2->next;
            }
        }
        ptr1 = ptr1->next;
    }
}

void dlist::deletesamenumber()
{
    ptr1 = head;
    while (ptr1 != NULL && ptr1->next != NULL)
    {
        ptr2 = ptr1;
        while (ptr2->next != NULL)
        {
            if (strcmp(ptr1->number, ptr2->next->number) == 0)
            {
                dup = ptr2->next;
                ptr2->next = ptr2->next->next;
                delete (dup);
            }
            else
            {
                ptr2 = ptr2->next;
            }
        }
        ptr1 = ptr1->next;
    }
}

void dlist::searchbyname(char na[10])
{
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(na, ptr->name) == 0)
        {
            cout << "NAME FOUND" << endl;
            cout << "CONTACT DETAILS ARE BELOW:\n"
                 << endl;
            cout << "\n\nNAME  ::\t" << ptr->name;
            cout << "\nNUMBER::\t+91-" << ptr->number;
            cout << "\nG-MAIL::\t" << ptr->gmail;

            // Add the call to displayContactHistory
            displayContactHistory(ptr->name);
        }
        ptr = ptr->next;
    }
}

void dlist::searchbynumber(char num[20])
{
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(num, ptr->number) == 0)
        {
            cout << "NUMBER FOUND\n"
                 << endl;
            cout << "CONTACT DETAILS ARE BELOW:\n"
                 << endl;
            cout << "\n\nNAME  ::\t" << ptr->name;
            cout << "\nNUMBER::\t+91-" << ptr->number;
            cout << "\nG-MAIL::\t" << ptr->gmail;

            // Add the call to displayContactHistory
            displayContactHistory(ptr->name);
        }
        ptr = ptr->next;
    }
}

void dlist::searchbygmail(char gm[20])
{
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(gm, ptr->gmail) == 0)
        {
            cout << "G-MAIL FOUND\n"
                 << endl;
            cout << "CONTACT DETAILS ARE BELOW:\n"
                 << endl;
            cout << "\n\nNAME  ::\t" << ptr->name;
            cout << "\nNUMBER::\t+91-" << ptr->number;
            cout << "\nG-MAIL::\t" << ptr->gmail;

            // Add the call to displayContactHistory
            displayContactHistory(ptr->name);
        }
        ptr = ptr->next;
    }
}

void dlist::update(char n[20])
{
    char ans;
    int c;
    ptr = head;
    while (ptr != NULL)
    {
        if (strcmp(n, ptr->name) == 0)
        {

            do
            {
                cout << "\nWHAT DO YOU WANT TO UPDATE?\n1.NAME\n2.PHONE NUMBER\n3.G-MAIL\n";
                cin >> c;
                const char *fieldName = getFieldName(c);
                const char *oldValue;

                switch (c)
                {
                case 1:
                    oldValue = ptr->name;
                    cout << "ENTER NEW-NAME=";
                    cin >> ptr->name;
                    break;
                case 2:
                    oldValue = ptr->number;
                    cout << "ENTER NEW PHONE-NUMBER?";
                    cin >> ptr->number;
                    while (strlen(ptr->number) != 10)
                    {
                        cout << "ENTER VALID NUMBER:";
                        cin >> ptr->number;
                    }
                    break;
                case 3:
                    oldValue = ptr->gmail;
                    cout << "ENTER NEW G-MAIL";
                    cin >> ptr->gmail;
                    break;
                default:
                    cout << "\nNO PROPER INPUT GIVEN.....\n";
                    continue;
                }

                // Call the updateHistory function
                updateHistory(ptr, fieldName, oldValue);

                cout << "Do you want to update another field? (y/n): ";
                cin >> ans;
            } while (ans == 'y');

            cout << "Contact updated successfully.\n\n";
            return;
        }
        ptr = ptr->next;
    }

    cout << "Contact not found.\n";
}

void dlist::saveToFile(const char *filename)
{
    ofstream outFile(filename);
    if (!outFile)
    {
        cerr << "Error opening file for writing.\n";
        return;
    }

    ptr = head;
    while (ptr != NULL)
    {
        outFile << ptr->name << " " << ptr->number << " " << ptr->gmail << endl;
        ptr = ptr->next;
    }

    outFile.close();
    cout << "Contacts saved to file successfully.\n";
}

void dlist::loadFromFile(const char *filename)
{
    ifstream inFile(filename);
    if (!inFile)
    {
        cerr << "Error opening file for reading.\n";
        return;
    }

    char name[30], number[50], gmail[40];
    while (inFile >> name >> number >> gmail)
    {
        temp = new dnode(name, number, gmail);
        if (head == NULL)
        {
            head = temp;
        }
        else
        {
            ptr = head;
            while (ptr->next != NULL)
            {
                ptr = ptr->next;
            }
            ptr->next = temp;
            temp->prev = ptr;
        }
    }

    inFile.close();
    cout << "Contacts loaded from file successfully.\n";
}

int main()
{
    dlist d1;

    // Load contacts from a file (if available)
    d1.loadFromFile("contacts.txt");

    int choice;
    char searchName[20], searchNumber[20], searchGmail[20], contactName[20];

    do
    {
        cout << "\n1. Insert a contact";
        cout << "\n2. Display all contacts";
        cout << "\n3. Sort contacts by name";
        cout << "\n4. Delete a contact by name";
        cout << "\n5. Delete contacts with the same name";
        cout << "\n6. Delete contacts with the same phone number";
        cout << "\n7. Delete contacts with the same G-mail";
        cout << "\n8. Search contact by name";
        cout << "\n9. Search contact by phone number";
        cout << "\n10. Search contact by G-mail";
        cout << "\n11. Update contact information";
        cout << "\n12. Save contacts to file";
        cout << "\n13. Exit";
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            d1.insert();
            break;
        case 2:
            d1.display();
            break;
        case 3:
            d1.sort();
            cout << "Contacts sorted successfully.\n";
            break;
        case 4:
            cout << "Enter the name to delete: ";
            cin >> contactName;
            d1.deletecontact(contactName);
            break;
        case 5:
            d1.deletesamename();
            cout << "Contacts with the same name deleted successfully.\n";
            break;
        case 6:
            d1.deletesamenumber();
            cout << "Contacts with the same phone number deleted successfully.\n";
            break;
        case 7:
            d1.deletesamegmail();
            cout << "Contacts with the same G-mail deleted successfully.\n";
            break;
        case 8:
            cout << "Enter the name to search: ";
            cin >> searchName;
            d1.searchbyname(searchName);
            break;
        case 9:
            cout << "Enter the phone number to search: ";
            cin >> searchNumber;
            d1.searchbynumber(searchNumber);
            break;
        case 10:
            cout << "Enter the G-mail to search: ";
            cin >> searchGmail;
            d1.searchbygmail(searchGmail);
            break;
        case 11:
            cout << "Enter the name to update: ";
            cin >> contactName;
            d1.update(contactName);
            break;
        case 12:
            d1.saveToFile("contacts.txt");
            break;
        case 13:
            cout << "Exiting the program.\n";
            break;
        default:
            cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 13);

    return 0;
}
                
