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

```
