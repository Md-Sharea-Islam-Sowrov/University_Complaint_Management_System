

#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>
#include <ctime>
#include <iomanip>
#include <limits>   
#include <map>

using namespace std;

const string DATA_FILE = "complaints.txt";

struct Complaint { 
    int id;
    string role;           
    string reporter_id;    
    string reporter_name;
    string category;       
    string subcategory;   
    string description;
    string timestamp;
    string status;         
};

string currentDateTime() {
    time_t now = time(nullptr);
    struct tm tstruct;
    char time[80];
#if defined(_WIN32) || defined(_WIN64)
    localtime_s(&tstruct, &now);
#else
    localtime_r(&now, &tstruct);
#endif

    strftime(time, sizeof (time), "%Y-%m-%d %H:%M:%S", &tstruct);
    return time;
}

vector<string> splitLine(const string &line, char delim='|') {
    vector<string> parts;
    string token;
    stringstream ss(line);
    while (getline(ss, token, delim)) {
        parts.push_back(token);
    }
    return parts;
}

string escapePipes(const string &s) {
  
    string out = s;
    for (int i =0; i<out.length(); i++) 
    {
        char &c = out[i];
        if (c == '|') c = '/';
    }
    return out;
}

Complaint lineToComplaint(const string &line) {
    vector<string> p = splitLine(line, '|');
    Complaint c;
    c.id = 0;
    if (p.size() >= 9) {
        c.id = stoi(p[0]);
        c.role = p[1];
        c.reporter_id = p[2];
        c.reporter_name = p[3];
        c.category = p[4];
        c.subcategory = p[5];
        c.description = p[6];
        c.timestamp = p[7];
        c.status = p[8];
    }
    return c;
}

string complaintToLine(const Complaint &c) {
    stringstream ss;
    ss << c.id << '|'
       << c.role << '|'
       << c.reporter_id << '|'
       << c.reporter_name << '|'
       << c.category << '|'
       << c.subcategory << '|'
       << escapePipes(c.description) << '|'
       << c.timestamp << '|'
       << c.status;
    return ss.str();
}

vector<Complaint> loadAllComplaints() {
    vector<Complaint> list;
    ifstream fin(DATA_FILE);
    if (!fin.is_open()) return list;
    string line;
    while (getline(fin, line)) {
        if (line.size() == 0) continue;
        Complaint c = lineToComplaint(line);
        if (c.id != 0) list.push_back(c);
    }
    fin.close();
    return list;
}

int getNextComplaintID(const vector<Complaint> &list) {
    int mx = 0;
    for (const auto &c : list) if (c.id > mx) mx = c.id;
    return mx + 1;
}

void saveAllComplaints(const vector<Complaint> &list) {
    ofstream fout(DATA_FILE, ios::trunc);
    for (const auto &c : list) {
        fout << complaintToLine(c) << '\n';
    }
    fout.close();
}

void appendComplaintToFile(const Complaint &c) {
    ofstream fout(DATA_FILE, ios::app);
    fout << complaintToLine(c) << '\n';
    fout.close();
}

void printComplaintTableHeader() {
    cout << left << setw(6) << "ID" << setw(10) << "Role" << setw(12) << "Reporter"
         << setw(14) << "Category" << setw(18) << "Subcategory" << setw(22) << "Time" 
         << setw(10) << "Status" << '\n';
    cout << string(100, '-') << '\n';
}

void printComplaintRow(const Complaint &c) {
    string rid = c.reporter_name + "(" + c.reporter_id + ")";
    if (rid.size() > 11) rid = rid.substr(0, 11) + ".";
    cout << left << setw(6) << c.id
         << setw(10) << c.role
         << setw(12) << rid
         << setw(14) << (c.category.size()>12 ? c.category.substr(0,12)+"." : c.category)
         << setw(18) << (c.subcategory.size()>16 ? c.subcategory.substr(0,16)+"." : c.subcategory)
         << setw(22) << (c.timestamp.size() > 20 ? c.timestamp.substr(0,20) : c.timestamp)
         << setw(10) << c.status << '\n';
}

void viewAllComplaints() {
    auto list = loadAllComplaints();
    if (list.empty()) {
        cout << "No complaints found.\n";
        return;
    }
    printComplaintTableHeader();
    for (const auto &c: list) printComplaintRow(c);
    cout << '\n';
}

void viewComplaintDetailed(const Complaint &c) {
    cout << "Complaint ID: " << c.id << "\n";
    cout << "Submitted by: " << c.reporter_name << " (" << c.reporter_id << ") [" << c.role << "]\n";
    cout << "Category: " << c.category << " -> " << c.subcategory << "\n";
    cout << "Time: " << c.timestamp << "\n";
    cout << "Status: " << c.status << "\n";
    cout << "Description:\n" << c.description << "\n";
}

void submitComplaint(const string &role) {
    cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    string reporter_id, reporter_name;
    cout << "Enter your ID (roll or faculty id): ";
    getline(cin, reporter_id);
    cout << "Enter your name: ";
    getline(cin, reporter_name);

    cout << "\nSelect main category:\n";
    cout << "1. Service (Classroom, Labs, Wi-Fi, Lift, Internet)\n";
    cout << "2. People (Faculty, Staff, Authority)\n";
    cout << "3. Student Behavior (Discipline, Class swapping, Cheating)\n";
    cout << "Choice: ";
    int catChoice;
    cin >> catChoice;
    cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');

    string category, subcategory;
    if (catChoice == 1) {
        category = "Service";
        cout << "Subcategory options:\n1. Classroom\n2. Computer Lab\n3. Physics Lab\n4. EEE Lab\n5. Wi-Fi/Internet\n6. Lift\nChoose number: ";
        int s; cin >> s; cin.ignore();
        switch(s) {
            case 1: subcategory="Classroom"; break;
            case 2: subcategory="Computer Lab"; break;
            case 3: subcategory="Physics Lab"; break;
            case 4: subcategory="EEE Lab"; break;
            case 5: subcategory="Wi-Fi/Internet"; break;
            case 6: subcategory="Lift"; break;
            default: subcategory="General Service"; break;
        }
    } else if (catChoice == 2) {
        category = "People";
        cout << "Subcategory options:\n1. Faculty\n2. Staff\n3. Authority\nChoose number: ";
        int s; cin >> s; cin.ignore();
        switch(s) {
            case 1: subcategory="Faculty"; break;
            case 2: subcategory="Staff"; break;
            case 3: subcategory="Authority"; break;
            default: subcategory="People-General"; break;
        }
    } else {
        category = "Student Behavior";
        cout << "Subcategory options:\n1. Irregular attendance\n2. Misbehavior\n3. Class swapping\n4. Cheating\nChoose number: ";
        int s; cin >> s; cin.ignore();
        switch(s) {
            case 1: subcategory="Irregular attendance"; break;
            case 2: subcategory="Misbehavior"; break;
            case 3: subcategory="Class swapping"; break;
            case 4: subcategory="Cheating"; break;
            default: subcategory="Behavior-General"; break;
        }
    }

    cout << "\nDescribe the issue (single paragraph). Press Enter when done:\n";
    string description;
    getline(cin, description);

    auto list = loadAllComplaints();
    int nextID = getNextComplaintID(list);

    Complaint c;
    c.id = nextID;
    c.role = role;
    c.reporter_id = reporter_id;
    c.reporter_name = reporter_name;
    c.category = category;
    c.subcategory = subcategory;
    c.description = description;
    c.timestamp = currentDateTime();
    c.status = "Pending";

    appendComplaintToFile(c);
    cout << "\nComplaint submitted successfully with ID: " << c.id << " at " << c.timestamp << "\n\n";
}

void studentPanel() {
    while (true) {
        cout << "\n--- Student Panel ---\n";
        cout << "1. Submit complaint\n2. View my complaints\n3. Search complaints by category\n4. Back to main menu\nChoose: ";
        int ch; cin >> ch;
        if (ch == 1) {
            submitComplaint("Student");
        } else if (ch == 2) {
            cin.ignore();
            string myid;
            cout << "Enter your ID (roll): ";
            getline(cin, myid);
            auto list = loadAllComplaints();
            vector<Complaint> mine;
            for (const auto &c : list) if (c.reporter_id == myid) mine.push_back(c);
            if (mine.empty()) cout << "No complaints found for ID " << myid << endl;
            else {
                printComplaintTableHeader();
                for (const auto &c : mine) printComplaintRow(c);
                cout << "\nEnter ID to view details or 0 to go back: ";
                int id; cin >> id;
                if (id != 0) {
                    for (const auto &c : mine) if (c.id == id) { viewComplaintDetailed(c); break; }
                }
            }
        } else if (ch == 3) {
            cin.ignore();
            cout << "Enter category to search (Service / People / Student Behavior): ";
            string cat; getline(cin, cat);
            auto list = loadAllComplaints();
            vector<Complaint> found;
            for (const auto &c : list) if (c.category == cat) found.push_back(c);
            if (found.empty()) cout << "No complaints found for category: " << cat << endl;
            else {
                printComplaintTableHeader();
                for (const auto &c : found) printComplaintRow(c);
            }
        } else break;
    }
}

void facultyPanel() {
    while (true) {
        cout << "\n--- Faculty Panel ---\n";
        cout << "1. Submit complaint about student or service\n2. View my complaints\n3. View complaints by section/semester (simple filter by reporter id prefix)\n4. Back to main menu\nChoose: ";
        int ch; cin >> ch;
        if (ch == 1) submitComplaint("Faculty");
        else if (ch == 2) {
            cin.ignore();
            string myid;
            cout << "Enter your Faculty ID: ";
            getline(cin, myid);
            auto list = loadAllComplaints();
            vector<Complaint> mine;
            for (const auto &c : list) if (c.reporter_id == myid) mine.push_back(c);
            if (mine.empty()) cout << "No complaints found for ID " << myid << endl;
            else {
                printComplaintTableHeader();
                for (const auto &c : mine) printComplaintRow(c);
                cout << "\nEnter ID to view details or 0 to go back: ";
                int id; cin >> id;
                if (id != 0) {
                    for (const auto &c : mine) if (c.id == id) { viewComplaintDetailed(c); break; }
                }
            }
        } else if (ch == 3) {
            cin.ignore();
            cout << "Enter section/semester keyword to filter reporter id or name (e.g., '2024', 'CSE'): ";
            string key; getline(cin, key);
            auto list = loadAllComplaints();
            vector<Complaint> found;
            for (const auto &c : list) if (c.reporter_id.find(key) != string::npos || c.reporter_name.find(key) != string::npos) found.push_back(c);
            if (found.empty()) cout << "No matches found.\n";
            else {
                printComplaintTableHeader();
                for (const auto &c : found) printComplaintRow(c);
            }
        } else break;
    }
}

void managementPanel() {
    const string adminPass = "admin123"; 
    cout << "\nEnter management password: ";
    string pass;
    cin >> pass;
    if (pass != adminPass) { cout << "Authentication failed.\n"; return; }

    while (true) {
        cout << "\n--- Management Panel ---\n";
        cout << "1. View all complaints\n2. Filter complaints by category / status\n3. View complaint details by ID\n4. Update complaint status (Pending -> Solved)\n5. Show statistics\n6. Back to main menu\nChoose: ";
        int ch; cin >> ch;
        if (ch == 1) {
            viewAllComplaints();
        } else if (ch == 2) {
            cin.ignore();
            cout << "Filter by (enter to skip): category (Service/People/Student Behavior): ";
            string category; getline(cin, category);
            cout << "Filter by status (Pending/Solved): ";
            string status; getline(cin, status);
            auto list = loadAllComplaints();
            vector<Complaint> filtered;
            for (const auto &c : list) {
                bool ok = true;
                if (category.size() && c.category != category) ok = false;
                if (status.size() && c.status != status) ok = false;
                if (ok) filtered.push_back(c);
            }
            if (filtered.empty()) cout << "No records for chosen filters.\n";
            else {
                printComplaintTableHeader();
                for (const auto &c : filtered) printComplaintRow(c);
            }
        } else if (ch == 3) {
            cout << "Enter complaint ID: ";
            int id; cin >> id;
            auto list = loadAllComplaints();
            bool found = false;
            for (const auto &c : list) if (c.id == id) { viewComplaintDetailed(c); found = true; break; }
            if (!found) cout << "Complaint not found.\n";
        } else if (ch == 4) {
            cout << "Enter complaint ID to update: ";
            int id; cin >> id;
            auto list = loadAllComplaints();
            bool found = false;
            for (auto &c : list) {
                if (c.id == id) {
                    found = true;
                    cout << "Current status: " << c.status << "\n";
                    cout << "Enter new status (Pending / Solved): ";
                    string ns; cin >> ns;
                    c.status = ns;
                    c.timestamp = currentDateTime();
                    break;
                }
            }
            if (!found) cout << "Complaint not found.\n";
            else {
                saveAllComplaints(list);
                cout << "Status updated and data saved.\n";
            }
        } else if (ch == 5) {
            auto list = loadAllComplaints();
            int total = list.size();
            int solved = 0;
            int pending = 0;
            map<string,int> catCount;
            for (const auto &c : list) {
                if (c.status == "Solved") ++solved;
                else ++pending;
                ++catCount[c.category];
            }
            cout << "\nStatistics:\n";
            cout << "Total complaints: " << total << "\n";
            cout << "Solved: " << solved << "\n";
            cout << "Pending: " << pending << "\n";
            cout << "By category:\n";
            for (auto &kv : catCount) cout << " " << kv.first << " : " << kv.second << '\n';
        } else break;
    }
}

void showMainMenu() {
    cout << "\n==== University Complaint Management System ====\n";
    cout << "Select your role:\n";
    cout << "1. Student\n2. Faculty\n3. Management\n4. Exit\nChoose: ";
}

int main() {
    cout << "Welcome to University Complaint Management System\n";
    while (true) {
        showMainMenu();
        int roleChoice;
        if (!(cin >> roleChoice)) break;
        if (roleChoice == 1) {
            studentPanel();
        } else if (roleChoice == 2) {
            facultyPanel();
        } else if (roleChoice == 3) {
            managementPanel();
        } else if (roleChoice == 4) {
            cout << "Exiting... Goodbye!\n";
            break;
        } else {
            cout << "Invalid choice, try again.\n";
        }
    }
    return 0;
}
