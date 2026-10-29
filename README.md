# Ex-1_INSTALLING-AND-RUNNING-APPLICATIONS-ON-ANDROID-STUDIO

# INSTALLING AND RUNNING APPLICATIONS ON ANDROID STUDIO

**Name:** Shreyeshkar Sekar  
**Register Number:** 212224220099  
**Department:** B.Tech Information Technology  

## 1. AIM

To develop, install, and execute a simple Bus Ticket Booking Application using Android Studio with Java as the programming language and XML for designing the user interface.

## 2. SOFTWARE REQUIREMENTS

1. Android Studio IDE
2. Java programming language
3. XML for user interface design
4. Android SDK
5. Android Emulator or Android mobile device
6. Windows operating system

## 3. ALGORITHM

1. Start the Android Studio application.
2. Create a new project using the Empty Views Activity template.
3. Set the project name as `TicketBookingApp` and select Java as the programming language.
4. Design the application interface using `activity_main.xml`.
5. Add input fields for passenger name and number of tickets.
6. Add dropdown menus to select the starting city and destination.
7. Add a Book Ticket button and a TextView to display the booking details.
8. Open `MainActivity.java` and connect the XML components using `findViewById()`.
9. Populate the dropdown menus with available cities.
10. Validate the passenger name, ticket quantity, and selected cities.
11. Calculate the total fare by multiplying the number of tickets by the fare per ticket.
12. Display the booking confirmation with passenger details, journey information, ticket quantity, and total fare.
13. Run the application on an Android Emulator.
14. Verify the application by entering sample details and clicking the Book Ticket button.
15. Stop the application.

## 4. PROGRAM

### A. XML Code – `activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F2F6FC"
    android:fillViewport="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="20dp">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="BUS TICKET BOOKING"
            android:textColor="#173B70"
            android:textSize="26sp"
            android:textStyle="bold"
            android:gravity="center"
            android:padding="12dp" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Passenger Name"
            android:textStyle="bold" />

        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter passenger name"
            android:inputType="textPersonName" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="From"
            android:textStyle="bold" />

        <Spinner
            android:id="@+id/spFrom"
            android:layout_width="match_parent"
            android:layout_height="50dp" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Destination"
            android:textStyle="bold" />

        <Spinner
            android:id="@+id/spTo"
            android:layout_width="match_parent"
            android:layout_height="50dp" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Number of Tickets"
            android:textStyle="bold" />

        <EditText
            android:id="@+id/etTickets"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter number of tickets"
            android:inputType="number"
            android:text="1" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Sample fare: Rs. 500 per ticket"
            android:paddingTop="10dp" />

        <Button
            android:id="@+id/btnBook"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="BOOK TICKET"
            android:layout_marginTop="20dp" />

        <TextView
            android:id="@+id/tvResult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="17sp"
            android:textColor="#173B70"
            android:padding="16dp"
            android:layout_marginTop="12dp" />

    </LinearLayout>
</ScrollView>
```

### B. Java Code – `MainActivity.java`

```java
// Keep the package declaration generated by Android Studio.

import android.os.Bundle;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etName, etTickets;
    Spinner spFrom, spTo;
    Button btnBook;
    TextView tvResult;

    String[] cities = {
        "Chennai", "Bangalore", "Coimbatore",
        "Madurai", "Salem", "Trichy"
    };

    final int farePerTicket = 500;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etTickets = findViewById(R.id.etTickets);
        spFrom = findViewById(R.id.spFrom);
        spTo = findViewById(R.id.spTo);
        btnBook = findViewById(R.id.btnBook);
        tvResult = findViewById(R.id.tvResult);

        ArrayAdapter<String> adapter = new ArrayAdapter<>(
            this,
            android.R.layout.simple_spinner_item,
            cities
        );

        adapter.setDropDownViewResource(
            android.R.layout.simple_spinner_dropdown_item
        );

        spFrom.setAdapter(adapter);
        spTo.setAdapter(adapter);

        btnBook.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                String name =
                    etName.getText().toString().trim();

                String ticketsText =
                    etTickets.getText().toString().trim();

                String from =
                    spFrom.getSelectedItem().toString();

                String to =
                    spTo.getSelectedItem().toString();

                if (name.isEmpty()) {
                    etName.setError("Enter passenger name");
                    return;
                }

                if (ticketsText.isEmpty()) {
                    etTickets.setError("Enter number of tickets");
                    return;
                }

                int tickets;

                try {
                    tickets = Integer.parseInt(ticketsText);
                } catch (NumberFormatException e) {
                    etTickets.setError("Enter a valid quantity");
                    return;
                }

                if (tickets < 1 || tickets > 100) {
                    etTickets.setError(
                        "Enter between 1 and 100 tickets"
                    );
                    return;
                }

                if (from.equals(to)) {
                    Toast.makeText(
                        MainActivity.this,
                        "Choose different cities",
                        Toast.LENGTH_SHORT
                    ).show();
                    return;
                }

                int totalFare = tickets * farePerTicket;

                String result =
                    "BOOKING CONFIRMED!\n\n"
                    + "Passenger: " + name + "\n"
                    + "From: " + from + "\n"
                    + "To: " + to + "\n"
                    + "Tickets: " + tickets + "\n"
                    + "Fare per ticket: Rs. " + farePerTicket + "\n"
                    + "Total Fare: Rs. " + totalFare;

                tvResult.setText(result);

                Toast.makeText(
                    MainActivity.this,
                    "Ticket booked successfully!",
                    Toast.LENGTH_SHORT
                ).show();
            }
        });
    }
}
```

## 5. PROCEDURE

1. Open Android Studio and create a new project named `TicketBookingApp`.
2. Select Java as the programming language and configure the Android SDK.
3. Open `activity_main.xml` and design the ticket booking interface.
4. Open `MainActivity.java` and implement the booking functionality.
5. Connect the XML components to Java using `findViewById()`.
6. Add the available cities to the starting-point and destination dropdown menus.
7. Implement validation for passenger details and ticket quantity.
8. Calculate the total fare based on the number of tickets selected.
9. Start an Android Emulator using Device Manager.
10. Click the Run button to install and execute the application.
11. Enter the passenger name, select the source and destination, enter the number of tickets, and click Book Ticket.
12. Verify that the booking confirmation and total fare are displayed.


## 6. OUTPUT :


<img width="1917" height="1145" alt="image" src="https://github.com/user-attachments/assets/b7564896-297d-41ea-8566-6e118362abd6" />


<img width="1600" height="999" alt="WhatsApp Image 2026-10-29 at 9 43 48 AM" src="https://github.com/user-attachments/assets/72512355-037c-4b20-b15f-d47fb01f2909" />



## 7. RESULT

Thus, the Bus Ticket Booking Application was developed using Android Studio, Java, and XML, and its execution was verified successfully on an Android Emulator.

