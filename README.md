# QR-Based-Bus-Ticketing-System
Developed a QR-based bus ticketing system to enhance hygiene and security in public transportation. Utilizing ESP32-S3 Eye, Arduino Uno, and ultrasonic sensors, the system detects approaching passengers and displays a QR code for ticket validation, allowing contactless interaction. 
[PYTHON CODE]
import kivy
import time
import qrcode
import firebase_admin
from firebase_admin import credentials,db
from kivy.lang import Builder
from kivy.app import App 
from kivy.uix.button import Button
from kivy.uix.gridlayout import GridLayout
from kivy.uix.screenmanager import Screen,ScreenManager
from kivy.clock import Clock
cred=credentials.Certificate({
  "type": "service_account",
  "project_id": "iotsecondsem",
  "private_key_id": "d2e2b223d7cd961102cb6cbc4c8698f9762b1c4c",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDYDf4FjZfvzk7s\nMtwLbRXNe9QsoJjpt4e6B58pgMBDhzpzrDnFp1fpr5lLnM9T8oTZqp1kJuxN7bm/\nYUBFtxemN6UrmCx+ag6MBEtnAis/RV3o8EGKVhnlOMn5jaCAHjVc87UZKjQjeBid\nvID6GfGtUxx1F6ncEO712sUGG780O4hcfoKMZMKlIVTrJu/mxbI4zlbGXhryjKQs\nbFRHtg6Zwpz1ViaM62PSvQyrmhSo4hIuXOO2TOnysy3E1PgeDRheJjszpS8CxRWi\nmwfrjLfOqxzAs77iO13hTz3vQQd36EFkQd7X0G0G+eIUUgmFBTNVnq8tF/ssjFYm\nHojiw9i1AgMBAAECggEAENJ4FJkbeyO6X+gu22HlOHBbixT/CvWMBxdIVHwunCNn\nkPGYNvRKY0rVmf5N40inAOaopg9kx0WK+KR7E2KH4ByFiwEod7hMxSihcVYJJX5C\na1xnkfyVvBQBn+Ff3ZHckE32bAzt7dQQQJnYhgCmoVrvBIvw4Q5deL2NqSXYa5nR\naoROElufUR5Z8HW2NPdioPHeoC/Cn1gbQJ5ePaUQy9QAk0J5a2ipy7Zu7KDYyOPH\nx/CRTS+O459y0HbdaOrTPSF9Jgf9kzRXYMso87YUMK4kIkPq/A78/ixcKenKd1K5\nPebCH/c+UrNctLCgTB0Ouhlb00dirM+dWE1rmI2SaQKBgQDtaHpr3DQ5ebzJiVv1\nnCqFE5ISKJJzJ15Gq+iW9VGJDxTDI95gaHmpAn7EWQ5VuIGGWRVvUmxMSTr072bz\n+ibK9/ZG6I+hPU7LA852PWTntjZZT2KY/Z/3Z0rfneOdNF4VInXBnGcKd6en/j5Z\nDXrvmm5g1wa5CKaRwYbNoLQRrQKBgQDo+WxSqDpioKq9lekyOJDWNgeTEL0NpOQC\niMYexkfArwyZk4Gyzt1BvJppOvC3I2u85mG+YHKndgEcgU/4oyqpj9dd6R9IAGPJ\nmph0yhXoUG2ZPh3s6iokIe15dUaRtBjTQ3C64IMR0MuHGwoextB/JggpeuslDrL5\nyhvk8ymUKQKBgDXNK5EuAhfUYtg5bMGodmpaGQxMbYPzNAZIBxnO1n99D3N5uXeX\ntZp7mkbc98atXY0YuybTQv+2yMmhR2+bDReKQiGnqoUb47NWVX+uQiPQw1hSCUIq\nmAn1Op6apW6G45teh9ksnJ1eqHwFvhNoXqfWE5WWpUthjn4RoX8QID6xAoGAbnlg\nIW3+iahQbqg4tYTXQYzLWLSWQXMQYBdFg3BYtAkN+4FT/ltT1gk+W2oEnYNhYmkI\nroMDu18ctcyoBGozH8bCxJh4Kedtajsx0ifF7ay92+31uNNtekbQWkj/VrZFE2Em\ngqdV38vXx1BOIzv5wGFje2/7M05eFk79nTqlW8kCgYEAmeb8a6jPsqbw46FX3t7w\nvz0cd4x+JjiwiOTBuBlMnKDWOWsebdlp7ad1IXTH8+pVRCxc6hPhPJmw6JfLjSBa\ndiTX0j7F74GNKUbz2v0FXkfodLEm5P05LzqifvKR2AHVozB9sAX+IxrdYPVKYCuT\nF8CwEBIFSMxvCmsVD9PpqBg=\n-----END PRIVATE KEY-----\n",
  "client_email": "firebase-adminsdk-x08bf@iotsecondsem.iam.gserviceaccount.com",
  "client_id": "106791249702028831504",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/firebase-adminsdk-x08bf%40iotsecondsem.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
})

firebase_admin.initialize_app(cred,{'databaseURL':'https://iotsecondsem-default-rtdb.firebaseio.com/'})

data_ref=db.reference('/PROFILE')
information = data_ref.get()
print(information)
class Profile(Screen):
    def submit(self):
        self.ids.name1.disabled=True
        self.ids.amount1.disabled=True
        self.ids.age1.disabled=True
        self.ids.gender1.disabled=True
        self.ids.adhar1.disabled=True

        n=self.ids.name1.text
        a=self.ids.age1.text
        g=self.ids.gender1.text
        ad=self.ids.adhar1.text
        am=self.ids.amount1.text
        
        data={'name':n,'age':a,'gender':g,'adhar':ad,'amount':am,'QR':"0"}
        entry=data_ref.push(data)
        key1 = entry.key
        print (key1)
        o=open(r"C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\usrname.txt",'w')
        o.write(key1)
        o.close()

    def update(self):
        self.ids.amount1.disable=False
        self.ids.amount1.text='broke'
    def add(self):
        money=int(self.ids.amount1.text)
        money=money+10  
        self.ids.amount1.text=str(money)
        o=open(r"C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\usrname.txt",'r')
        use=o.read()
        creden=data_ref.get()[use]
        print(creden)
        creden['amount']=str(money)
        data_ref.update({use:creden})


    pass
class Details(Screen):
    def on_enter(self):
        o=open(r"C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\usrname.txt",'r')
        idd=o.read()
        aayush= data_ref.get()[idd]
        a=aayush['amount']
        print(a)
        self.ids.diya.text=str(a)

    def cal(self):
        st=self.ids.start.text
        s=self.ids.stop.text
        places=['HNLU','SEC27','SEC29','CBD','SOUTH BLOCK','TELIBANDA','GHADI CHOWK']
        p= places.index(st)
        q= places.index(s)
        final_price= (abs(p-q))*10
        self.ids.price.text=str(final_price)

    def qrgen1(self):
        qr= qrcode.QRCode(
            version=1,
            error_correction=qrcode.constants.ERROR_CORRECT_L,
            box_size=10,
            border=4
        )   
        o=open(r"C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\usrname.txt",'r')
        use=o.read()
        data=use
        qr.add_data(data)
        qr.make(fit=True)

        img= qr.make_image(fill_color='black',black_color='white')
        img.save("diyush.png")
        self.manager.switch_to(qrgen())

        
    #getting location of amount
        ref= '/PROFILE/'+data+'/amount'
        qrref='/PROFILE/'+data+'/QR'
    #getting the value from cloud
        ref1= db.reference(ref)
        ref2=db.reference(qrref)

        ref2.set("1")
        current=ref1.get()

        deduct=self.ids.price.text

        cloud_amt=int(current)-int(deduct)

        ref1.set(cloud_amt)

    def switch_to_qrgen(self,dt):
            self.manager.switch_to(qrgen())




        

    pass
class qrgen(Screen):    
    def switch_to_Details(self):
        self.manager.current = 'details'

    def handle_change(self, event):  # Accept event parameter
        if event.data == 0:
            print("changed to :",event.data)
            self.ids.qr_sub.disabled=False

    def __init__(self, **kwargs):
        super(qrgen, self).__init__(**kwargs)

        # Open the file to read the username
        with open(r'C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\usrname.txt', 'r') as f:
            username = f.read().strip()  # Read and strip any whitespace

        # Construct Firebase URL
        url = "/PROFILE/" + username + "/QR"

        # Get the database reference
        ref = db.reference(url)

        ref.listen(self.handle_change)



class WindowManager(ScreenManager):
    def switch_to_qrgen(self, dt):
        self.current = 'QR'
    

kv= Builder.load_file('Diyaapp.kv')
class DiyaApp(App):
    def build(self):
        return kv
    
if __name__=='__main__':
    DiyaApp().run()

[KV FILE]
WindowManager:
    Profile:
    Details:
    qrgen:

<Profile>:
    name:'profile'
    GridLayout:
        rows:3
        Label:
            text:'PROFILE'
        GridLayout:
            rows:5
            cols:2
            Label:
                text:'name'
            TextInput:
                id:name1
                multiline:False
            Label:
                text:'age'
            TextInput:
                id:age1
                multiline:False
            Label:
                text:'gender'
            TextInput:
                id:gender1
                multiline:False
            Label:
                text:'Adhar no'
            TextInput:
                id:adhar1
                multiline:False
            Label:
                text:'Amount'
            GridLayout:
                rows:1
                cols:2
                TextInput:
                    id:amount1
                    multiline:False
                Button:
                    text:"+"
                    on_press:root.add()
                
        GridLayout:
            rows:1
            cols:2
            Button: 
                text:'SUBMIT'
                on_press:root.submit()
                on_release: app.root.current ='details'
            Button:
                text:'UPDATE'
                on_press:root.update()
                on_release: app.root.current ='details'

    
<Details>:
    name:'details'
    GridLayout:
        rows:5
        cols:1
        GridLayout:
            rows:1
            cols:3
            Label:
                text:'amount'
            Label:
                id:diya
                text:''
            Button:
                text:'Profile'
        GridLayout:
            rows:1
            cols:2
            Label:
                text:'START'
            TextInput:
                id:start
                multiline:False
                
        GridLayout:
            rows:1
            cols:2
            Label:
                text:'STOP'
            TextInput:
                id:stop
                multiline:False
                
        GridLayout:
            rows:1
            cols:2
            Label:
                text:'PRICE'
            TextInput:
                id:price
                multiline:False
                disabled:True
        GridLayout:
            rows:1
            col:2
            Button:
                text:'DONE'
                on_press:root.cal()
            Button:
                text:'QR CODE'
                on_press:root.qrgen1()
               



<qrgen>:
    name:'QR'   
    GridLayout:
        rows:2
        padding:10
        
        Image:
            id: qr_code
            source:r'C:\Users\KIIT\AppData\Local\Programs\Python\Python37-32\diyush.png'
        Button:
            id:qr_sub
            text:"DONE!!"
            bold:True
            disabled:True
            size_hint:(1,0.3)
            on_press:root.switch_to_Details()


[HARDWARE PYTHON CODE]
import serial
import time

# Connect to Arduino via serial port
ser = serial.Serial('COM3', 9600)  # Replace 'COM6' with your Arduino's serial port

# Wait for Arduino to initialize
time.sleep(2)

# Function to set servo position for a specific pin
def set_servo_position(pin, angle):
    # Send the pin number and angle to Arduino
    command = f"{pin}:{angle}\n"
    ser.write(command.encode())
    print(f"Set servo position: Pin {pin}, Angle {angle}")
    time.sleep(1)

# Example usage
servo_pin = 6  # Specify the pin number your servo is connected to

for i in range(0, 10):
    set_servo_position(servo_pin, -90)  # Set the servo to -90 degrees
    time.sleep(1)
    set_servo_position(servo_pin, 90)   # Set the servo to 90 degrees
    time.sleep(1)

# Close the serial connection
ser.close()
