```cpp

#include <iostream>
#include <vector>
#include <fstream>
#include <sstream>

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

int main(){
}

```
