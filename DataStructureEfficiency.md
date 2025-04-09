# Test for time efficiency

```cpp

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
    string schoolFile = "Illinois_Schools.csv";

    vector<vector<string>> schoolsData = CSVReader::readCSV(filename);

    Timer timerI; //Test insert into linked list
    for (const auto& row : schoolsData) {
        School school(row[0], row[1], row[2], row[3], row[4]);
        schoolList.insertLast(school);
    }
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
