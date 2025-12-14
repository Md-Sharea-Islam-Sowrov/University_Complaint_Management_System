# EduComplainPortal
#include<iostream>
using namespace std;

int WinMain()
{
    int step;

    cout<<"Welcome to our complain system.\n\n";

    cout<<"1. Student\n";
    cout<<"2. Faculty\n";
    cout<<"3. Authority\n\n";

    cout<<"Select your role: ";
    cin>>step;
    cout<<endl;

    // ------------------ STUDENT SECTION ------------------
    if(step == 1)
    {
        cout<<"Education complain portal options:\n";
        cout<<"1. Services\n";
        cout<<"2. Faculty\n";
        cout<<"3. Staff\n\n";

        cout<<"Select your option: ";
        cin>>step;
        cout<<endl;

        // ---------- SERVICES ----------
        if(step == 1)
        {
            cout<<"Available Services:\n";
            cout<<"1. Classroom\n";
            cout<<"2. Computer Lab\n";
            cout<<"3. Physics Lab\n";
            cout<<"4. EEE Lab\n";
            cout<<"5. Lift\n";
            cout<<"6. WiFi\n\n";

            cout<<"Select your option: ";
            cin>>step;
            cout<<endl;

            // CLASSROOM
            if(step == 1)
            {
                cout<<"Classrooms:\n";
                cout<<"1. Room 401\n";
                cout<<"2. Room 402\n";
                cout<<"3. Room 403\n";
                cout<<"4. Room 404\n";
                cout<<"5. Room 405\n\n";

                cout<<"Select a classroom: ";
                cin>>step;
                cout<<endl;

                if(step == 1)
                {
                    cout<<"Services in Room 401:\n";
                    cout<<"1. AC\n";
                    cout<<"2. Projector\n";
                    cout<<"3. Light/Fan\n";
                }
            }
            else if(step == 2)
            {
                cout<<"Computer Labs:\n";
                cout<<"406\n407\n408\n409\n410\n";
            }
            else if(step == 3)
            {
                cout<<"Physics Lab: 412\n";
            }
            else if(step == 4)
            {
                cout<<"EEE Lab: 416\n";
            }
            else if(step == 5)
            {
                cout<<"Lift 1\nLift 2\n";
            }
            else if(step == 6)
            {
                cout<<"Submit your WiFi complain:\n";
            }
        }

         // ---------- FACULTY ----------
        else if(step == 2)
        {
            cout<<"Faculty List:\n";
            cout<<"1. Md. Raihanullah\n";
            cout<<"2. Md. Shamsuddoha Alam\n";
            cout<<"3. Md. Ruhul Amin\n";
            cout<<"4. Zarin Tasnim Rothy\n";
        }
        // ---------- STAFF ----------
        else if(step == 3)
        {
            cout<<"Staff List:\n";
            cout<<"1. Md. Mojammel\n";
            cout<<"2. Md. Fayaj\n";
        }
    }
     // ---------- STAFF ----------
    else if(step == 3)
    {
        cout<<"Staff List:\n";
        cout<<"1. Md. Mojammel\n";
        cout<<"2. Md. Fayaj\n";
    }
       

    return 0;
}
