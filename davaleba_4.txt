import mysql.connector
from dateutil.relativedelta import relativedelta
import datetime
from datetime import datetime
import random as ran
from faker import Faker
faker = Faker()

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  password="",
  database="mydata"
)

mycursor = mydb.cursor()

def datagenerator(gender):
    arr = []
    name = ''
    lastname = ''
    if gender == "Male":
        name = faker.first_name_male()
        lastname = faker.last_name_male()
    elif gender == "Female":
        name = faker.first_name_female()
        lastname = faker.last_name_female()
    date = faker.date_of_birth()
    age = relativedelta(datetime.now(),date).years
    reg_date = datetime.now()
    password = faker.password(length=8,special_chars=True,upper_case=True)
    gender = gender
    arr.extend([name,lastname,age,date,reg_date,password,gender])
    
    return arr
def insert_method(data):
    sql = "INSERT INTO users(Name,Lastname,Age, Date,Reg_date,Password,Gender) VALUES (%s,%s,%s,%s,%s,%s,%s)"
    mycursor.executemany(sql,data)
    mydb.commit()
    return mycursor.rowcount
def write_data():
    count = 0
    data = []
    for i in range(6,5000001):
        if len(data)>=10000:
            count += insert_method(data)
            print(count,"row inserted")
            data = []
        data.append(datagenerator(ran.choice(['Female','Male'])))


sql = "INSERT INTO users (ID,Name,Lastname,Age,Date,Reg_date,Password,Gender) VALUES (%s,%s,%s,%s,%s,%s,%s,%s)"
val = [
      (1,"Luka", "Nebieridze",20,datetime.date(2000, 1, 19),datetime.date(2020, 10, 28),12345,"Male"),
      (2,"Tata", "Nebieridze",18,datetime.date(2002, 4, 23),datetime.date(2020, 10, 28),54321,"Female"),
      (3,"Gregoris", "Tsouloukidis",20,datetime.date(2014, 7, 4),datetime.date(2020, 10, 27),10234,"Male"),
      (4,"Johny", "Jonjoliani",30,datetime.date(2015,7,4),datetime.date(2020, 10, 27),21234,"Male"),
      (5,"Jenifer", "Cook",30,datetime.date(2017,7,4),datetime.date(2020, 10, 20),21534,"Female")
      ]
mycursor.executemany(sql,val)

mydb.commit()

print(mycursor.rowcount, "record inserted.")
def threeValue(ID):
    mycursor = mydb.cursor()
    sql = "SELECT Age,Date,Reg_date,Gender FROM users WHERE ID = %s"
    mycursor.execute(sql, (ID,))
    myresult = mycursor.fetchall()
    for x in myresult:
        print(x)
ID_1 = 1
ID_2 = 2
ID_3 = 3
threeValue(ID_1)
threeValue(ID_2)
threeValue(ID_3)
def allValue(ID):
    mycursor = mydb.cursor()
    sql = "SELECT * From users WHERE ID=%s"
    mycursor.execute(sql,(ID,))
    myresult = mycursor.fetchall()
    for x in myresult:
        print(x)
ID_1 = 1
ID_2 = 2
allValue(ID_1)
allValue(ID_2)
def all_great():
    mycursor = mydb.cursor()
    sql = "SELECT * From users WHERE ID>1 and ID<4"
    mycursor.execute(sql)
    myresult = mycursor.fetchall()
    for x in myresult:
        print(x)
all_great()
def all_great_or_equal():
    mycursor = mydb.cursor()
    sql = "SELECT * From users WHERE ID<2 or ID>=4"
    mycursor.execute(sql)
    myresult = mycursor.fetchall()
    for x in myresult:
        print(x)
all_great_or_equal()
def date(d):
    mycursor = mydb.cursor()
    sql = "SELECT * FROM users WHERE Date = %s"
    mycursor.execute(sql, (d,))
    myresult = mycursor.fetchall()
    for x in myresult:
        print(x)
dat = 2014-7-4
date(dat)















    
    
    






