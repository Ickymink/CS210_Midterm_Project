# Cristian Rodriguez

```cpp

#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>

using namespace std;

class CSVReader {
public:
    static vector<vector<string>> readCSV(const string& filename) {
        ifstream file(filename);
        vector<vector<string>> data;
        string line, word;

        if (!file.is_open()) {
            cerr << "Error: Could not open file " << filename << endl;
            return data;
        }

        while (getline(file, line)) {
            stringstream ss(line);
            vector<string> row;
            while (getline(ss, word, ',')) {
                row.push_back(word);
            }
            data.push_back(row);
        }
        file.close();
        return data;
    }
};

struct School {
    string name;
    string address;
    string city;
    string state;
    string county;
    School* next;

    School(string n, string a, string c, string s, string co) {
        name = n;
        address = a;
        city = c;
        state = s;
        county = co;
        next = nullptr;
    }
};

class SchoolList {
private:
    School* head;
public:
    SchoolList() {
        head = nullptr;
    }

    void insertFirst(School school) {
        School* newSchool = new School(school.name, school.address, school.city, school.state, school.county);
        newSchool->next = head;
        head = newSchool;
    }

    void insertLast(School school) {
        School* newSchool = new School(school.name, school.address, school.city, school.state, school.county);
        if (!head) {
            head = newSchool;
            return;
        }
        School* temp = head;
        while (temp->next) {
            temp = temp->next;
        }
        temp->next = newSchool;
    }

    void deleteByName(string name) {
        if (!head) {
            cout << "The list is empty." << endl;
            return;
        }

        School* current = head;
        School* prev = nullptr;

        while (current && current->name != name) {
            prev = current;
            current = current->next;
        }

        if (!current) {
            cout << "School not found: " << name << endl;
            return;
        }

        if (current == head) {
            head = head->next;
        }

        else if (!current->next) {
            prev->next = nullptr;
        }

        else {
            prev->next = current->next;
        }

        delete current;
        cout << "Deleted: " << name << endl;
    }

    void findByName(string name) {
        School* temp = head;
        while (temp) {
            if (temp->name == name) {
                cout << "School found: " << temp->name << ", " << temp->address << ", "
                << temp->city << ", " << temp->state << ", " << temp->county << endl;
                return;
            }
            temp = temp->next;
        }
        cout << "The school is not found." << endl;
    }

    void display() {
        if (!head) {
            cout << "No Schools in the list." << endl;
            return;
        }
        School* temp = head;
        while (temp) {
            cout << temp->name << ", " << temp->address << ", "
            << temp->city << ", " << temp->state << ", " << temp->county << endl;
            temp = temp->next;
        }
    }
};

int main() {
    SchoolList schoolList;
    string filename = "Illinois_Peoria_Schools.csv";

    vector<vector<string>> schoolsData = CSVReader::readCSV(filename);
    for (const auto& row : schoolsData) {
        School school(row[0], row[1], row[2], row[3], row[4]);
        schoolList.insertLast(school);
    }

    int choice;
    string name;
    do {
        cout << "School Management System" << endl;
        cout << "1. Display Schools" << endl;
        cout << "2. Find a School by Name" << endl;
        cout << "3. Delete a School by Name" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                schoolList.display();
                break;
            case 2:
                cout << "Enter school name to find: ";
                getline(cin, name);
                schoolList.findByName(name);
                break;
            case 3:
                cout << "Enter school name to delete: ";
                getline(cin, name);
                schoolList.deleteByName(name);
                break;
            case 4:
                cout << "Exiting program." << endl;
                break;
            default:
                cout << "Invalid choice. Try again." << endl;
                break;
        }
        cout << endl;
    } while (choice != 4);

    return 0;
}

```
