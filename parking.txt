/// design  parking lot 
#include <iostream>
//# include<bits/stdc++.h>
# include<thread> 
# include<vector>
# include<map>
# include<queue>

using namespace std;
string owner_id;
string owner_pass;
void f_owner();
void f_main_gate();
void f_entry_gate();
void f_exit_gate();
void f_section_incharge();
string two_wheeler_id, two_wheeler_pass, four_wheeler_id, four_wheeler_pass, ten_wheeler_id, ten_wheeler_pass;
vector<map<string, string>> section_incharge_id_pass(4);
vector<map<pair<int, string>, string>> entry_gate_incharge(4);
vector<map<pair<int, string>, string>> exit_gate_incharge(4);
string main_gate_entry_id, main_gate_entry_pass, main_gate_exit_id, main_gate_exit_pass;
queue<pair<int, string>> entry_register_main_gate, exit_register_main_gate;
typedef priority_queue <pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> mn_pq;
vector<string>   cash_payment_vec;

//   list out all the needs 
/*
(1)  in one parking lot we have more than one section that is
      (i)   one for two wheeler
      (ii) another for four wheeler
    and also we can increase the section in future for example in future we may add a section for ten wheeler vehicle


(2) now each section of  parking slot will have two thing
        (i) some entry point can be more than one
        (ii) some exit point can be more than one


(3)   and then we want to provide service to the vehicle










*/

////   implementtation of stand class 


 vector<mn_pq> vec2;
 map<string, pair<int, int>> mp2;
 
 
class twowheeler_stand {
private:
    int gate_no;
    
public:
    twowheeler_stand(int gate) {
        gate_no = gate;
    }
    static void initialise() {
        mn_pq   q;
        vec2.resize(5,q);
        for (int j = 1; j <= 100; j++) {
            for (int k = 1; k <= 100; k++) {
                if (j <= 25)
                    vec2[1].push({ abs(j - 20) + abs(k),{j,k} });
                else if (j <= 50) {
                    vec2[2].push({ abs(j - 40) + abs(k),{j,k} });
                }
                else if (j <= 75) {
                    vec2[3].push({ abs(j - 60) + abs(k),{j,k} });
                }
                else {
                    vec2[4].push({ abs(j - 80) + abs(k),{j,k} });
                }
            }
        }
    }
    void f_entry_gate_request(string& van_no, int& r, int& c) {
        if (vec2[gate_no].size() == 0) {
            cout << "no_space_available" << endl;
            return;
        }
        r = vec2[gate_no].top().second.first;
        c = vec2[gate_no].top().second.second;
        mp2[van_no] = { r,c };
        vec2[gate_no].pop();
    }
    void f_exit_gate_request(string& van_no) {
        vec2[gate_no].push({ abs(mp2[van_no].first - (gate_no * 20)) + abs(mp2[van_no].second),{mp2[van_no].first,mp2[van_no].second} });
    }

};
  vector<mn_pq> vec4;
 map<string, pair<int, int>> mp4;
class fourwheeler_stand {
private:
   
    int gate_no;
    //static int    arr[100][100];
    
public:
    fourwheeler_stand(int no) {
        gate_no = no;
    }
    static void initialise() {
        mn_pq q;
        vec4.resize(5,q);
        for (int j = 1; j <= 100; j++) {
            for (int k = 1; k <= 100; k++) {
                if (j <= 25)
                    vec4[1].push({ abs(j - 20) + abs(k),{j,k} });
                else if (j <= 50) {
                    vec4[2].push({ abs(j - 40) + abs(k),{j,k} });
                }
                else if (j <= 75) {
                    vec4[3].push({ abs(j - 60) + abs(k),{j,k} });
                }
                else {
                    vec4[4].push({ abs(j - 80) + abs(k),{j,k} });
                }
            }
        }

    }
    void f_entry_gate_request(string& van_no, int& r, int& c) {
        if (vec4[gate_no].size() == 0) {
            cout << "no_space_available" << endl;
            return;
        }
        r = vec4[gate_no].top().second.first;
        c = vec4[gate_no].top().second.second;
        mp4[van_no] = { r,c };
        vec4[gate_no].pop();
    }
    void f_exit_gate_request(string& van_no) {
        vec4[gate_no].push({ abs(mp4[van_no].first - (gate_no * 20)) + abs(mp4[van_no].second),{mp4[van_no].first,mp4[van_no].second} });
    }

};
vector<mn_pq> vec10;
    ///static int    arr[100][100];
map<string, pair<int, int>> mp10;
class tenwheeler_stand {
private:
    int gate_no;
      
public:
    tenwheeler_stand(int gate){
        gate_no = gate;
    }
    static void initialise() {
       mn_pq q;
       vec10.resize(5,q);

        for (int j = 1; j <= 100; j++) {
            for (int k = 1; k <= 100; k++) {
                if (j <= 25)
                    vec10[1].push({ abs(j - 20) + abs(k),{j,k} });
                else if (j <= 50) {
                    vec10[2].push({ abs(j - 40) + abs(k),{j,k} });
                }
                else if (j <= 75) {
                    vec10[3].push({ abs(j - 60) + abs(k),{j,k} });
                }
                else {
                    vec10[4].push({ abs(j - 80) + abs(k),{j,k} });
                }
            }
        }


    }
    void f_entry_gate_request(string& van_no, int& r, int& c) {
        if (vec10[gate_no].size() == 0) {
            cout << "no_space_available" << endl;
            return;
        }
        r = vec10[gate_no].top().second.first;
        c = vec10[gate_no].top().second.second;
        mp10[van_no] = { r,c };
        vec10[gate_no].pop();
    }

    void f_exit_gate_request(string& van_no) {
        vec10[gate_no].push({ abs(mp10[van_no].first - (gate_no * 20)) + abs(mp10[van_no].second),{mp10[van_no].first,mp10[van_no].second} });

    }

};

///////////    

//    implementation of payment class 



class payment {
protected:
    string payer_name, van_no;int  section_no;
    int amount_paid;
    string payment_type;
public:
    payment(string& name, string& no, int & sec_no, int& amount) {
        payer_name = name;
        van_no = no;
        section_no = sec_no;
        amount_paid = amount;
    }
    virtual void set_payment_type(string& type) = 0;



};
class cash_payment :public payment {
private:
    int exit_gate_no;
    string cash_taker_id;
  
public:
    cash_payment(int& section_no, int& exit_gate_no, string& cash_taker_id, string& payer_name, string& van_no, int& amt)
        : payment(payer_name, van_no, section_no, amt) {

        this->exit_gate_no = exit_gate_no;
        this->cash_taker_id = cash_taker_id;
    }
    void set_payment_type(string& type) override {
        this->payment_type = type;
    }
    void store(cash_payment &obj){
        string str=obj.payer_name;
        str+=",";
        str+=obj.van_no;
        str+=",";
        str+=obj.amount_paid;
        str+=",";
        cash_payment_vec.push_back(str);
    }





};
// class  upi_payment:public payment{

// };


////// payment class implementation ends here 


class owner {
private:
    string id, pass;
public:
    owner(string& id1, string& pass1) {
        this->id = id1;
        this->pass == pass1;
    }
    void f_handle_owner_query() {
        //   add section incharge 
        /*
        (1)   two wheeler
        (2)   four wheeler
        (3)   more than ten wheeler
        */
        while (1) {
            int x; cin >> x;
            if (x == 1) {
                string id, pass;
                cin >> id >> pass;
                two_wheeler_id = id;
                two_wheeler_pass = pass;
                section_incharge_id_pass[1][id] = pass;

            }
            else if (x == 2) {
                string id, pass;
                cin >> id >> pass;
                four_wheeler_id = id;
                four_wheeler_pass = pass;
                section_incharge_id_pass[2][id] = pass;
            }
            else if (x == 3) {
                string id, pass;
                cin >> id >> pass;
                ten_wheeler_id = id;
                ten_wheeler_pass = pass;
                section_incharge_id_pass[3][id] = pass;
            }
            else if (x == 4) {
                int n;
                string id, pass;
                cin >> n >> id >> pass;
                if (n == 1) {
                    main_gate_entry_id = id;
                    main_gate_entry_pass = pass;
                }
                else {
                    main_gate_exit_id = id;
                    main_gate_exit_pass = pass;
                }

            }
            else {
                cout << "enter a valid key " << endl;
            }

        }



    }

};
class section_incharge {
private:
    int section_no;
    string id, pass;
public:
    section_incharge(int n, string s1, string s2) {
        section_no = n;
        id = s1; pass = s2;
    }
    void f_add_entry_incharge() {
        int gate_no; string id, pass;
        cin >> gate_no >> id >> pass;
        entry_gate_incharge[section_no][{gate_no, id}] = pass;
    }
    void f_add_exit_incharge() {
        int gate_no; string id, pass;
        cin >> gate_no >> id >> pass;
        exit_gate_incharge[section_no][{gate_no, id}] = pass;
    }
    void f_handle_section_incharge() {
        // add entry  gate incharge 
        // add exit gate incharge
        while (1) {
            cout << "Enter 1 for entry and 2 for exit " << endl;
            int x;
            cin >> x;
            if (x == 1) {
                f_add_entry_incharge();
            }
            else if (x == 2) {
                f_add_exit_incharge();
            }
            else {
                cout << "enter valid key" << endl;
            }
        }


    }


};
void f_owner() {
    while (1) {

        cout << "Enter valid id and pass" << endl;
        string id, pass; cin >> id >> pass;
        if (owner_id == id && owner_pass == pass) {
            owner   obj_owner(id, pass);
            obj_owner.f_handle_owner_query();
        }
        else {
            cout << "invalid id and password combination " << endl;

        }


    }
}
void f_section_incharge() {
    while (1) {
        cout << "Enter section number id and pass" << endl;
        int section_number; string id, pass;
        cin >> section_number >> id >> pass;
        if (section_incharge_id_pass[section_number].count(id) && section_incharge_id_pass[section_number][id] == pass) {
            section_incharge obj1(section_number,id, pass);
            obj1.f_handle_section_incharge();
        }
        else {
            cout << "invalid id pass" << endl;
        }



    }

}
class main_gate {
private:
    string id, pass;
public:
    main_gate(string& s1, string& s2) {
        id = s1; pass = s2;

    }
    void f_main_gate_entry() {
        while (1) {
            int n;
            string van_no;
            cin >> n >> van_no;
            entry_register_main_gate.push({ n,van_no });

        }


    }
    void f_main_gate_exit() {
        while (1) {
            int n;
            string van_no;
            cin >> n >> van_no;
            exit_register_main_gate.push({ n,van_no });
        }
    }

};
void f_main_gate() {
    while (1) {
        int n;
        cin >> n;
        string id, pass;
        cin >> id >> pass;
        if (n == 1) {
            if (id == main_gate_entry_id && pass == main_gate_entry_pass) {
                main_gate obj1(id, pass);
                obj1.f_main_gate_entry();
            }
            else {
                cout << "Enter valid id and pass" << endl;

            }

        }
        else {
            if (id == main_gate_exit_id && pass == main_gate_exit_pass) {
                main_gate obj1(id, pass);
                obj1.f_main_gate_exit();

            }
            else {
                cout << "Enter a valid id and pass" << endl;
            }
        }
    }
}

map<string, pair<int, int>> vehicle_entered;
vector<mn_pq> seat_detail(12);


class entry_gate {
private:
    int section_no; string id, pass; int gate_no;
public:
    entry_gate(int no, string & id1, string & pass1,int &gate) {
        section_no = no;
        id = id1;
        pass = pass1;
        gate_no = gate;
    }
    void f_handle_entry_gate() {
        //  implement it 
        while (1) {
            string van_no;
            cin >> van_no;
            int r = -1, c = -1;
            if (section_no == 1) {
                twowheeler_stand obj(gate_no);
                obj.f_entry_gate_request(van_no, r, c);
            }
            else if (section_no == 2) {
                fourwheeler_stand  obj(gate_no);
                obj.f_entry_gate_request(van_no, r, c);
            }
            else {
                tenwheeler_stand   obj(gate_no);
                obj.f_entry_gate_request(van_no, r, c);
            }


        }
    }
};
class exit_gate {
private:
    int section_no; string id, pass; int gate_no;
public:
    exit_gate(int& no, string& id1, string& pass1, int& gate) {
        section_no = no;
        id = id1;
        pass = pass1;
        gate_no = gate;
    }
    void f_handle_exit_gate() {
        // implement it 
        while (1) {
            string type = "cash";
            string  payer_name, van_no;int  amt;
            cin >> payer_name >> van_no >> amt;
            cash_payment obj1(section_no, gate_no, id, payer_name, van_no, amt);
           obj1.store(obj1);
            if (section_no == 1) {

                twowheeler_stand obj(gate_no);
                obj.f_exit_gate_request(van_no);
            }
            else if (section_no == 2) {
                fourwheeler_stand obj(gate_no);
                obj.f_exit_gate_request(van_no);
            }
            else {
                tenwheeler_stand obj(gate_no);
                obj.f_exit_gate_request(van_no);
            }

        }


    }

};
void f_entry_gate() {
    while (1) {
        int section_no;
        string id, pass; int gate_no;
        cin >> section_no >> id >> pass >> gate_no;
        if (section_no < 1 && section_no>3) {
            cout << "invalid data enter valid data" << endl;
        }
        else if (entry_gate_incharge[section_no].count({gate_no,id}) && entry_gate_incharge[section_no][{gate_no, id}] == pass) {
            // entry_gate_incharge
            entry_gate obj1(section_no, id, pass, gate_no);
            obj1.f_handle_entry_gate();

        }
        else {
            cout << "invalid data enter valid data" << endl;
        }
    }
}
void f_exit_gate() {
    while (1) {
        int section_no; int gate_no; string id, pass;
        cin >> section_no >> id >> pass;
        if (section_no < 1 && section_no>3) {
            cout << "invalid data enter valid data" << endl;
        }
        else if (exit_gate_incharge[section_no].count({gate_no,id}) && exit_gate_incharge[section_no][{gate_no, id}] == pass) {
            // exit_gate_incharge
            exit_gate obj1(section_no, id, pass, gate_no);
            obj1.f_handle_exit_gate();

        }
        else {
            cout << "invalid data enter valid data" << endl;
        }

    }
}

int main()

{
    cin >> owner_id >> owner_pass;
    /*
          identify      role
          1             owner
          2             main_gate
          3             entry_gate of any section
          4             exit gate of any  section
          5             section_incharge



    */
    twowheeler_stand :: initialise();
    fourwheeler_stand:: initialise();
    tenwheeler_stand:: initialise();

    while (1) {
        //  when vehicle  is entering at the main gate 
        int identify;
        cin >> identify;
        if (identify == 1) {
            f_owner();
        }
        else if (identify == 2) {
            f_main_gate();
        }
        else if (identify == 3) {
            f_entry_gate();
        }
        else if (identify == 4) {
            f_exit_gate();
        }
        else if (identify == 5) {
            f_section_incharge();
        }
        else {
            cout << "enter your valid identity " << endl;
        }



    }



    return 0;
}