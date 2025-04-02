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

int main(){
}

```
