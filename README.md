# Login-and-registrarion

import re
import csv

Email_re= re.compile(r'([A-Za-z0-9]+[.-_])*[A-Za-z0-9]+@[A-Za-z0-9-]+(\.[A-Z|a-z]{2,})+')
Pas_re = re.compile(r'^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{5,}$')

# checking user name with constraints
def isValidEmail(Email):
    if re.fullmatch(Email_re, Email):
      return True
    else:
      return False
# checking password with constraints
def isValidPas(Pas):
    if len(Pas) < 16 and len(Pas) > 5 and re.fullmatch(Pas_re, Pas):
      return True
    else:
      return False
# registering  new user with valid user name and password
def register():
    print('\nRegister New User\n')
    Email = input('Email id: ')
    if isValidEmail(Email):
        password = input('password: ')
        if isValidPas(password):
            write_file(Email, password)
            print('\nUser Registered Successfully')
        else:
            print('\nInvalid Password, Please Try Again...')
    else:
        print('\nInvalid Username, Please Try Again...')
  # login to existing id

def login():
    Email = input('Email id: ')
    auth  = False
    if isValidEmail(Email):
        password = input('password: ')
        if isValidPas(password):
            if search_file(Email, password):
                print ('\nUser Logged In Successfully...')
            else:
                print('\nUser Not Found !!!')
                register()
        else:
            print('\nInvalid Password, Please Try Again...')
    else:
        print('\nInvalid Username, Please Try Again...')
# checking for forget password
def forget_Pas():
    Email = input('Email id: ')
    if isValidEmail(Email):
        if search_Pas(Email):
            print ('\nUser Logged In Successfully...')
        else:
            print('\nUser Not Found !!!')
            register()
    else:
        print('\nInvalid Username, Please Try Again...')

def search_file(Email, Pas , mode = 'r', encoding = 'UTF8', newline = ''):
    with open('users.csv', mode, encoding = encoding, newline = newline) as f:
        reader = csv.reader(f)
        for row in reader:
            if Email == row[0] and Pas == row[1]:
                return True
        return False

def search_Pas(email, mode = 'r', encoding = 'UTF8', newline = ''):
    with open('users.csv', mode, encoding = encoding, newline = newline) as f:
        reader = csv.reader(f)
        for row in reader:
            if email == row[0]:
                print('\nPassword for '+ email + ' is '+ row[1])
                return True
        return False
        
# CSV util module
def write_file(Email, Pas, mode = 'a', encoding = 'UTF8', newline = ''):
    with open('users.csv', mode, encoding = encoding, newline = newline) as f:
        writer = csv.writer(f)
        writer.writerow([Email, Pas])

if __name__ == "__main__":
   
    try:
        while True:
            print(''' \nPlease select an option \n
                1. Register
                2. Login
                3. Forget Password
                ''')
            option = int(input())
            if option == 1:
                register()
            elif option == 2:
                login()
            elif option == 3:
                forget_Pas()
            else:
                print("Invalid")
    except:
        print("Error")
