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
    School* left;
    School* right;

    School(string n, string a, string c, string s, string co) {
        name = n;
        address = a;
        city = c;
        state = s;
        county = co;
        left = nullptr;
        right = nullptr;
    }
};

class SchoolBST {
private:
    School* root;
    School* insertNode(School* node, School* newSchool) {
        if (node == nullptr) return newSchool;
        if (newSchool->name < node->name) node->left = insertNode(node->left, newSchool);
        else node->right = insertNode(node->right, newSchool);
        return node;
    }
    School* findNode(School* node, string value) {
        if (node == nullptr) {
            cout << "School not found: " << value << endl;
            return nullptr;
        }
        if (node->name == value){
            cout << "School found: " << node->name << ", " << node->address << ", "
                << node->city << ", " << node->state << ", " << node->county << endl;
            return node;
        }
        return (value < node->name) ? findNode(node->left, value) : findNode(node->right, value);
    }
    School* findMin(School* node) {
        while (node->left != nullptr) node = node->left;
        return node;
    }
    School* deleteNode(School* node, string value) {
        if (node == nullptr)
            return nullptr;
        if (value < node->name) {
            node->left = deleteNode(node->left, value);
        }
        else if (value > node->name) {
            node->right = deleteNode(node->right, value);
        }
        else {
            cout << "Deleting: " << node->name << endl;

            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }

            if (node->left == nullptr) {
                School* temp = node->right;
                delete node;
                return temp;
            }
            if (node->right == nullptr) {
                School* temp = node->left;
                delete node;
                return temp;
            }

            School* temp = findMin(node->right);
            node->name = temp->name;
            node->address = temp->address;
            node->city = temp->city;
            node->state = temp->state;
            node->county = temp->county;
            node->right = deleteNode(node->right, node->name);
        }
        return node;
    }
    void inOrder(School* node) {
        if (node == nullptr)
            return;
        inOrder(node->left);
        cout << node->name << ", " << node->address << ", "
        << node->city << ", " << node->state << ", " << node->county << endl;
        inOrder(node->right);
    }
    void preorder(School* node) {
        if (node == nullptr)
            return;
        cout << node->name << ", " << node->address << ", "
        << node->city << ", " << node->state << ", " << node->county << endl;
        preorder(node->left);
        preorder(node->right);
    }
    void postorder(School* node) {
        if (node == nullptr)
            return;
        postorder(node->left);
        postorder(node->right);
        cout << node->name << ", " << node->address << ", "
        << node->city << ", " << node->state << ", " << node->county << endl;
    }
    void deleteTree(School* node) {
        if (node == nullptr) return;
        deleteTree(node->left);
        deleteTree(node->right);
        delete node;
    }
};

```
