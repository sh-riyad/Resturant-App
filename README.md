# Resturant-App
//This is a console based application for my university project
#include <bits/stdc++.h>
using namespace std;
#define n endl
int TotalFood = 0, TotalEmployee = 0, TotalOrder = 0, TotalAmount = 0;
bool EmployeeFlag = false, UserFlag = false;
vector<pair<int, int>> CoustomerOrder;
string username, password;
class Menu
{
public:
    int SerialNumber;
    string FoodName;
    int FoodPrice;
};
class Employee
{
public:
    string EmployeeName;
    string EmployeeSex;
    string EmployeeNumber;
    string EmployeeEmail;
    string EmployeeAddress;
    string EmployeeDateofBirth;
    string EmployeePassword;
};
class Menu menu[100];
class Employee employee[100];

void ReadEmployeeDataFromFile()
{
    // cin.ignore();
    ifstream EmployeeData("EmployeeData.txt");
    if (EmployeeData.is_open())
    {
        while (!EmployeeData.eof())
        {
            // cin.ignore();
            EmployeeData >> employee[TotalEmployee].EmployeeName >> employee[TotalEmployee].EmployeeSex >> employee[TotalEmployee].EmployeeNumber >> employee[TotalEmployee].EmployeeEmail >> employee[TotalEmployee].EmployeeAddress >> employee[TotalEmployee].EmployeeDateofBirth >> employee[TotalEmployee].EmployeePassword;
            TotalEmployee++;
        }
    }
    else
        cout << "Unable to Open Employee Database.\n";
}
void ShowEmployeeData()
{
    if (TotalEmployee == 0)
        cout << "There are no employee.\n";
    else
    {
        for (int j = 0; j < TotalEmployee; j++)
        {
            cout << "Name : " << employee[j].EmployeeName << n << "Gender : " << employee[j].EmployeeSex << n << "Contact Number : " << employee[j].EmployeeNumber << n << "Email : " << employee[j].EmployeeEmail << n << "Address : " << employee[j].EmployeeAddress << n << "Date of Birth : " << employee[j].EmployeeDateofBirth << n; // employee[j].EmployeePassword
        }
    }
}

void ReadMenuFromFile()
{
    ifstream FoodMenu("FoodMenu.txt");
    if (FoodMenu.is_open())
    {

        while (!FoodMenu.eof())
        {

            FoodMenu >> menu[TotalFood].SerialNumber >> menu[TotalFood].FoodName >> menu[TotalFood].FoodPrice;
            TotalFood++;
        }
    }
    else
        cout << "Unable to open Menu" << n;
}
bool VerifyEmployee()
{
    for (int j = 0; j < TotalEmployee; j++)
        return (employee[j].EmployeeName == username && employee[j].EmployeePassword == password);
}
void ShowMenu()
{
    if (TotalFood == 0)
        cout << "No Food Avable." << n;
    else
    {
        cout << endl;
        cout << "Food Code"
             << "\t"
             << "Food Price"
             << "\t"
             << "Food Name"
             << "\n\n";
        for (int j = 0; j < TotalFood; j++)
        {
            cout << menu[j].SerialNumber << "\t\t" << menu[j].FoodPrice << "\t\t" << menu[j].FoodName << n;
        }
    }
}
void OrderSummary()
{
    if (TotalOrder == 0)
        cout << "0 Food Order"
             << "\n\n";
    else
    {
        cout << "\t\t"
             << "Order Summary\n";
        cout << "Food Name"
             << "\t"
             << "Food Price"
             << " "
             << "Food Quantity" << endl;

        for (int j = 0; j < TotalOrder; j++)
        {
            cout << menu[CoustomerOrder[j].first - 1].FoodName << "\t\t\t" << menu[CoustomerOrder[j].first - 1].FoodPrice << "\t  " << CoustomerOrder[j].second << "\n";
            if (!EmployeeFlag)
                TotalAmount += menu[CoustomerOrder[j].first - 1].FoodPrice * CoustomerOrder[j].second;
        }
        cout << "Total Amount to Pay : " << TotalAmount << " BDT only." << n;
        if (UserFlag)
        {
            cout << "\n\tPress Any Key to Enter Main Menu\n";
            cin.ignore();
            cin.ignore();
        }
    }
}
void OrderList()
{
    if (TotalOrder > 0)
        cout << "You Already have some pending orders\n\n";
    cout << "\n";
    cout << "Food Code"
         << " "
         << "<space>"
         << " "
         << "Food Quantity"
         << " "
         << "(When your order is complete type 0 0)" << endl;
    while (true)
    {
        int Number, Quantity;
        cin >> Number >> Quantity;
        if (Number == 0 && Quantity == 0)
            break;
        else
        {
            CoustomerOrder.push_back({Number, Quantity});
            TotalOrder++;
        }
    }
    UserFlag = true;
    OrderSummary();
}
void UserAccess()
{
    ShowMenu();
    int choice;
    cout << endl
         << "1. Order Food\n"
         << endl
         << "2. Exit\n"
         << endl;
    cin >> choice;
    if (choice == 1)
    {
        OrderList();
    }
}
void EmployeeAccess()
{
    int Choice;
    // ShowEmployeeData();
    cout << "Enter your username: ";
    cin >> username;
    cout << "Enter your Password: ";
    cin >> password;
    if (VerifyEmployee())
    {
        cout << "1. View Full Enployee List" << endl
             << "2. View Food Request" << endl
             << "3. See Food Order" << endl;
        cout << "Enter Your Choice : ";
        cin >> Choice;
        if (Choice == 1)
            ShowEmployeeData();
        else if (Choice == 2)
            cout << "No Food Request" << endl;
        else if (Choice == 3)
        {
            EmployeeFlag = true;
            UserFlag = false;
            OrderSummary();
            int Done;
            cout << "1. Mark as Done" << n << "Enter your Choice : ";
            cin >> Done;
            if (Done == 1)
            {
                TotalOrder = 0;
                CoustomerOrder.clear();
            }
        }
    }
    else
        cout << "Wrong Login Information\n"
             << n;
}
int main()
{
    ReadEmployeeDataFromFile();
    ReadMenuFromFile();
    int UserChoice;

    cout << "Wellcome" << n;
    while (1)
    {
        cout << "1. Access as a Employees" << n << "2. Access as a Coustomer" << n << "Enter your choice : ";
        cin >> UserChoice;
        if (UserChoice == 1)
        {
            cin.ignore();
            EmployeeAccess();
        }

        else if (UserChoice == 2)
        {
            UserAccess();
        }
        else
        {
            cout << "Wrong Input" << endl;
            // cout << "1. Access as a Employees" << n << "2. Access as a Coustomer" << n << "Enter your choice : ";
        }
    }

    return 0;
}
