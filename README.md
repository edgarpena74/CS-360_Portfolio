# CS-360 Project Three: Inventory App

The goal of this project was to design and develop a fully functional Android mobile application that demonstrates key concepts of mobile app development, user interface design, and database integration. The Inventory App allows users to log in, create and manage an inventory list, and receive SMS alerts when stock levels are low or out of stock. The app was built using java and SQLite, and it follows Android development best practices for readability, usability, and efficiency.

This app was designed to meet user needs for quick, simple inventory tracking that can be managed directly from a mobile device. It helps users organize essential product information, such as name, amount, price, and location, while providing optional text message notifications for restocking reminders.

----
### Technologies Used
[SQLite](https://sqlite.org/docs.html)

[SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper)

[Android Studio](https://developer.android.com/studio)

[Android Studio Debug](https://developer.android.com/studio/debug)

[Android Studio Logcat](https://developer.android.com/studio/debug/logcat)

[Android](https://developer.android.com/develop)

[Java](https://docs.oracle.com/en/java/)



--------
#### User Requirements and UI Design

The main user needs addressed by this app were:

-   The ability to store and manage inventory items easily.
    
-   Quick visual access to all inventory data in a grid view.
    
-   Optional SMS notifications for low or out-of-stock items.
    
To meet these needs, the app includes the following screens and features:

-   **Login and Registration Screen** - Allows users to create new accounts or log in to existing ones.
    
-   **Home Screen** - Displays a personalized welcome message, lets users enable or disable SMS alerts, and provides access to the inventory.
    
-   **Inventory Screen** - Displays all inventory items in a grid, allowing users to add, update, delete, and view their items quickly. This screen will also send a simulated SMS notification when the item inventory reaches below a certain threshold. 
    
Each screen was designed with a user-centered UI in mind, using clear labels, intuitive navigation, and consistent layouts. The interface remained readable and straightforward, allowing users to perform all main actions easily and without confusion.

-----------

#### Coding Process

When coding the app, I followed a structured, modular approach by separating responsibilities into repositories and helper classes, including UserRepository, InventoryRepository, and AppDatabaseHelper. This separation made the code easier to maintain and debug.

Throughout development, each feature was tested incrementally before moving on to the next stage. Consistent commenting and descriptive naming conventions were applied to ensure the code remained easy to follow. These practices not only make the project easier for other developers to understand and modify, but also support future improvements for a potential production-ready version that could be published on the Google Play Store.

------
#### Testing and Debugging

Testing was a continuous part of the development process. I used two main tools for debugging and verification:

1.  **Android Studio Terminal** – Used to test database functionality directly and verify that tables and data were being created, stored, and retrieved correctly.
    
2.  **Logcat** – Served as the main debugging tool to track runtime errors, app crashes, and invalid user sessions. Logcat helped me identify issues such as missing intent extras and null values, and confirm that methods were executing as expected.

In addition to using these tools, I also tested the app using the Android Emulator to confirm layout performance, button functionality, and SMS permission handling. Testing ensured that the app worked smoothly across different Android API levels and that error handling worked correctly for invalid or unexpected user input.

------
#### Challenges 

One of the main challenges was ensuring that the app handled SMS permissions correctly while still functioning when permissions were denied. To solve this, I implemented conditional checks that allowed the app to continue running normally even if the SMS feature was disabled.
```
//-----------------------------  
// TEST SMS BUTTON  
//-----------------------------  
buttonTestSms.setOnClickListener(v -> {  
  
  // Grab latest values from UI  
  boolean smsEnabledNow = switchEnableSms.isChecked();  
    String phoneNow = editTextPhoneNumber.getText().toString().trim();  
  
   // Keep them (so DB stays in sync with what we just tested)  
userRepository.updateUserSmsSettings(username, phoneNow, smsEnabledNow);  
  
   if (smsEnabledNow) {  
  String phoneDisplay = phoneNow.isEmpty() ? "(000)000-0000" : phoneNow;  
        Toast.makeText(this,  
                "SMS Notification sent to " + phoneDisplay +  
                        "\nYour test message was successful.",  
                Toast.LENGTH_LONG).show();  
    } else {  
  Toast.makeText(this,  
                "SMS alerts are not currently enabled. Please change your settings.",  
                Toast.LENGTH_LONG).show();  
    }  
});
```

Another challenge involved maintaining database persistence across activities. To overcome this, consistent user IDs and repository methods were used to connect login data and inventory records without losing state when switching screens. The user’s information, including ID, phone number, and SMS settings, was passed between activities using intents to preserve session context:

```
//||||||||||||||||||||||||
// FROM HomeActivity.java
//||||||||||||||||||||||||

//------------------------
// OPEN INVENTORY BUTTON  
//-----------------------  
buttonOpenInventory.setOnClickListener(v -> {  
  // Get freshest values from UI first  
  String phoneNow = editTextPhoneNumber.getText().toString().trim();  
    boolean smsEnabledNow = switchEnableSms.isChecked();  
  
    // Save them before moving screens  
  userRepository.updateUserSmsSettings(username, phoneNow, smsEnabledNow);  
  
    // Launch InventoryActivity WITH full context  
  Intent intent = new Intent(HomeActivity.this, InventoryActivity.class);  
    // which user's inventory  
  intent.putExtra("EXTRA_USER_ID", userId);  
    // phone for mock SMS  
  intent.putExtra("EXTRA_PHONE", phoneNow);  
    // whether alerts are allowed or not  
  intent.putExtra("EXTRA_SMS_ENABLED", smsEnabledNow);  
    startActivity(intent);
```
```
//|||||||||||||||||||||||||||||
// FROM InventoryActivity.java
//|||||||||||||||||||||||||||||

//-----------------------------  
// onCreate  
//-----------------------------  
@Override  
protected void onCreate(Bundle savedInstanceState) {  
  super.onCreate(savedInstanceState);  
    setContentView(R.layout.activity_inventory);  
	  
	  //*************************************
    // Pull session info from HomeActivity 
    //************************************* 
  userId = getIntent().getIntExtra("EXTRA_USER_ID", -1);  
    phoneNumber = getIntent().getStringExtra("EXTRA_PHONE");  
    smsEnabled = getIntent().getBooleanExtra("EXTRA_SMS_ENABLED", false);
```
By maintaining these user IDs and related context data across activities, the app ensured that the correct inventory data and settings were always loaded for the logged-in user, providing a smooth and reliable experience.

-----
#### Areas of Success
 The project successfully met all functional and design requirements outlined in the course. Key achievements include implementing a functional and user-friendly interface, integrating a persistent **SQLite** database with full **CRUD functionality**, and managing **SMS permissions** in compliance with Android’s security standards. The development process strengthened understanding of modular code design, UI construction using **Java**, and the use of debugging tools such as **Logcat** and the **Android terminal**. 

----

## Closing Statement

This project significantly contributed to my growth as a developer. This opened the door to new possibilities for creating apps for both personal use and for the Google Play Store. The most Important thing I learned from this project is how to build a stable project and the steps I need to take to ensure everything functions correctly for the user.

