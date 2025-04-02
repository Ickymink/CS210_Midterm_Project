```cpp

#include <iostream>
#include <vector>
#include <fstream>
#include <sstream>

using namespace std;

struct School {
    string name;
    string address;
    string city;
    string state;
    string county;

    School(string n, string a, string c, string s, string co) {
        name = n;
        address = a;
        city = c;
        state = s;
        county = co;
    }
};

class SchoolHashTable {
private:
    static const int TABLE_SIZE = 10007;
    vector<list<School>> table;

    int hashFunction(string key, int tableSize) {
        int hash = 0;
        for (char ch : key) {
            hash += ch;
        }
        return hash % tableSize;
    }

public:
    SchoolHashTable() : table(TABLE_SIZE) {}

    void insert(School school) {
        int index = hashFunction(school.name, TABLE_SIZE);
        table[index].push_back(school);
    }

    void deleteByName(string name) {
        int index = hashFunction(name, TABLE_SIZE);
        auto& chain = table[index];
        for (auto i = chain.begin(); i != chain.end(); i++) {
            if (i->name == name) {
                chain.erase(i);
                cout << "Deleted: " << name << endl;
                return;
            }
        }
        cout << "School not found: " << name << endl;
    }

    void findByName(string name) {
        int index = hashFunction(name, TABLE_SIZE);
        auto& chain = table[index];

        for (auto& school : chain) {
            if (school.name == name) {
                cout << "School found: " << school.name << ", " << school.address << ", "
                << school.city << ", " << school.state << ", " << school.county << endl;
                return;
            }
        }
        cout << "School not found: " << name << endl;
    }

    void display() {
        for (int i = 0; i < table.size(); i++) {
            if (!table[i].empty()) {
                for (auto& school : table[i]) {
                    cout << school.name << ", " << school.address << ", "
                    << school.city << ", " << school.state << ", " << school.county << endl;
                }
            }
        }
    }
};

void loadSchools(SchoolHashTable& schoolTable, const string& filename) {
    ifstream file(filename);
    if (!file.is_open()) {
        cerr << "Error: Could not open file " << filename << endl;
        return;
    }

    string line;
    getline(file, line); //Skips the header

    while (getline(file, line)) {
        stringstream ss(line);
        string name, address, city, state, county;

        getline(ss, name, ',');
        getline(ss, address, ',');
        getline(ss, city, ',');
        getline(ss, state, ',');
        getline(ss, county, ',');

        schoolTable.insert(School(name, address, city, state, county));
    }
    file.close();
}

int main(){
    SchoolHashTable schoolTable;
    loadSchools(schoolTable, "Illinois_Peoria_Schools.csv");

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
                schoolTable.display();
                break;
            case 2:
                cout << "Enter school name to find: ";
                getline(cin, name);
                schoolTable.findByName(name);
                break;
            case 3:
                cout << "Enter school name to delete: ";
                getline(cin, name);
                schoolTable.deleteByName(name);
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
