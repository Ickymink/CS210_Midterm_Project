# Test for time efficiency(LinkedList)

```cpp

void loadAndInsert(const string& filename, SchoolList& list) {
    ifstream file(filename);
    if (!file.is_open()) {
        cerr << "Failed to open file: " << filename << endl;
        return;
    }

    string line;
    getline(file, line); // skip header

    while (getline(file, line)) {
        stringstream ss(line);
        string name, address, city, state, county;

        getline(ss, name, ',');
        getline(ss, address, ',');
        getline(ss, city, ',');
        getline(ss, state, ',');
        getline(ss, county, ',');

        list.insertLast(School(name, address, city, state, county));
    }

    file.close();
}

void writeCSV(const string& filename, const string& structure, const string& schoolFile,
    double insertTime, double deleteTime, double findTime) {

    bool fileExists = ifstream(filename).good();
    ofstream file;

    file.open(filename, ios::app);

    if (!fileExists) {
        file << "SchoolFile,Structure,InsertTime,DeleteTime,FindTime\n";
    }

    file << schoolFile << "," << structure << ","
         << insertTime << "," << deleteTime << "," << findTime << "\n";

    file.close();
}

int main() {
    SchoolList schoolList;
    string schoolFile = "USA_Schools.csv";

    Timer timerI; //Test insert into linked list
    loadAndInsert(schoolFile, schoolList);
    double time_insert = timerI.get_time();

    Timer timerD; // Test delete by name
    schoolList.deleteByName("WOODRUFF CAREER & TECH CENTER");
    double time_dbn = timerD.get_time();

    Timer timerF; // Test find by name
    schoolList.findByName("VON STEUBEN MIDDLE SCHOOL");
    double time_find = timerF.get_time();

    writeCSV("benchmark_results.csv", "Linked List", schoolFile, time_insert, time_dbn, time_find);

    return 0;
}

```

# Test for time efficiency(BST)

```cpp

void loadAndInsert(const string& filename, SchoolBST& bst) {
    ifstream file(filename);
    if (!file.is_open()) {
        cerr << "Failed to open file: " << filename << endl;
        return;
    }

    string line;
    getline(file, line); // skip header

    while (getline(file, line)) {
        stringstream ss(line);
        string name, address, city, state, county;

        getline(ss, name, ',');
        getline(ss, address, ',');
        getline(ss, city, ',');
        getline(ss, state, ',');
        getline(ss, county, ',');

        School* school = new School(name, address, city, state, county);
        bst.insert(school);
    }

    file.close();
}

void writeCSV(const string& filename, const string& structure, const string& schoolFile,
    double insertTime, double deleteTime, double findTime) {

    bool fileExists = ifstream(filename).good();
    ofstream file;

    file.open(filename, ios::app);

    if (!fileExists) {
        file << "SchoolFile,Structure,InsertTime,DeleteTime,FindTime\n";
    }

    file << schoolFile << "," << structure << ","
         << insertTime << "," << deleteTime << "," << findTime << "\n";

    file.close();
}

int main() {
    SchoolBST schoolTree;
    string schoolFile = "USA_Schools.csv";

    Timer timerI; //Test insert into the BST
    loadAndInsert(schoolFile, schoolTree);
    double time_insert = timerI.get_time();

    Timer timerD; // Test delete by name
    schoolTree.deleteByName("WOODRUFF CAREER & TECH CENTER");
    double time_dbn = timerD.get_time();

    Timer timerF; // Test find by name
    schoolTree.find("VON STEUBEN MIDDLE SCHOOL");
    double time_find = timerF.get_time();

    writeCSV("benchmark_results.csv", "BST", schoolFile, time_insert, time_dbn, time_find);

    return 0;
}

```

# Test for time efficiency(Hash Table)

```cpp

void loadSchools(SchoolHashTable& schoolTable, const string& filename) {
    ifstream file(filename);
    if (!file.is_open()) {
        cerr << "Error: Could not open file " << filename << endl;
        return;
    }

    string line;
    getline(file, line);

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

void writeCSV(const string& filename, const string& structure, const string& schoolFile,
    double insertTime, double deleteTime, double findTime) {

    bool fileExists = ifstream(filename).good();
    ofstream file;

    file.open(filename, ios::app);

    if (!fileExists) {
        file << "SchoolFile,Structure,InsertTime,DeleteTime,FindTime\n";
    }

    file << schoolFile << "," << structure << ","
         << insertTime << "," << deleteTime << "," << findTime << "\n";

    file.close();
}

int main(){
    SchoolHashTable schoolTable;
    string schoolFile = "USA_Schools.csv";
    loadSchools(schoolTable, schoolFile);

    Timer timerI; //Test insert into Hash Table
    loadSchools(schoolTable, "Illinois_Peoria_Schools.csv");
    double time_insert = timerI.get_time();

    Timer timerD; // Test delete by name
    schoolTable.deleteByName("WOODRUFF CAREER & TECH CENTER");
    double time_dbn = timerD.get_time();

    Timer timerF; // Test find by name
    schoolTable.findByName("VON STEUBEN MIDDLE SCHOOL");
    double time_find = timerF.get_time();

    writeCSV("benchmark_results.csv", "Hash Table", schoolFile, time_insert, time_dbn, time_find);


    return 0;
}

```
