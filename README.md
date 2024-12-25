# train-management-system
PROGRAM:
package trainmanagementsystem;
import java.util.*;

class Passenger {
    private String name;
    private int age;
    private String seatType;

    public Passenger(String name, int age, String seatType) {
        this.name = name;
        this.age = age;
        this.seatType = seatType;
    }

    public String getDetails() {
        return "Name: " + name + ", Age: " + age + ", Seat Type: " + seatType;
    }
}

class Train {
    private String trainName;
    private String route;
    private String schedule;
    private double price;

    public Train(String trainName, String route, String schedule, double price) {
        this.trainName = trainName;
        this.route = route;
        this.schedule = schedule;
        this.price = price;
    }

    public String getDetails() {
        return "Train Name: " + trainName + ", Route: " + route + ", Schedule: " + schedule + ", Price: $" + price;
    }

    public String getRoute() {
        return route;
    }
}

class Booking {
    private Passenger passenger;
    private Train train;

    public Booking(Passenger passenger, Train train) {
        this.passenger = passenger;
        this.train = train;
    }

    public String getDetails() {
        return passenger.getDetails() + ", Train: " + train.getDetails();
    }
}

class TrainManagementSystem {
    private List<Train> trains = new ArrayList<>();
    private List<Passenger> passengers = new ArrayList<>();
    private List<Booking> bookings = new ArrayList<>();
    private Map<String, Integer> seatAvailability = new HashMap<>();

    public void addTrain(Train train) {
        trains.add(train);
        seatAvailability.put(train.getRoute(), 100); // Assume 100 seats per train
    }

    public void displayTrains() {
        System.out.println("Available Trains:");
        for (Train train : trains) {
            System.out.println(train.getDetails());
        }
    }

    public Train getTrain(String route) {
        for (Train train : trains) {
            if (train.getRoute().equalsIgnoreCase(route)) {
                return train;
            }
        }
        return null;
    }

    public void bookTicket(String name, int age, String seatType, String route) {
        Train train = getTrain(route);
        if (train != null && seatAvailability.get(route) > 0) {
            Passenger passenger = new Passenger(name, age, seatType);
            passengers.add(passenger);
            bookings.add(new Booking(passenger, train));
            seatAvailability.put(route, seatAvailability.get(route) - 1);
            System.out.println("Ticket booked successfully!");
        } else {
            System.out.println("No seats available for this route.");
        }
    }

    public void cancelTicket(String name, String route) {
        Iterator<Booking> iterator = bookings.iterator();
        while (iterator.hasNext()) {
            Booking booking = iterator.next();
            if (booking.getDetails().contains(name) && booking.getDetails().contains(route)) {
                iterator.remove();
                seatAvailability.put(route, seatAvailability.get(route) + 1);
                System.out.println("Ticket canceled successfully!");
                return;
            }
        }
        System.out.println("No booking found for the given details.");
    }

    public void displayBookings() {
        System.out.println("Current Bookings:");
        for (Booking booking : bookings) {
            System.out.println(booking.getDetails());
        }
    }

    public void displayAvailableSeats(String route) {
        if (seatAvailability.containsKey(route)) {
            System.out.println("Available Seats for " + route + ": " + seatAvailability.get(route));
        } else {
            System.out.println("No train found for this route.");
        }
    }
}
package trainmanagementsystem;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        TrainManagementSystem system = new TrainManagementSystem();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nTrain Management System:");
            System.out.println("1. Add Train");
            System.out.println("2. Display Trains");
            System.out.println("3. Book Ticket");
            System.out.println("4. Cancel Ticket");
            System.out.println("5. Display Bookings");
            System.out.println("6. Display Available Seats");
            System.out.println("7. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    // Add a train
                    System.out.print("Enter Train Name: ");
                    String trainName = scanner.nextLine();
                    System.out.print("Enter Route (e.g., A-B): ");
                    String route = scanner.nextLine();
                    System.out.print("Enter Schedule (e.g., 10:00 AM): ");
                    String schedule = scanner.nextLine();
                    System.out.print("Enter Price: ");
                    double price = scanner.nextDouble();
                    system.addTrain(new Train(trainName, route, schedule, price));
                    System.out.println("Train added successfully!");
                    break;

                case 2:
                    // Display trains
                    system.displayTrains();
                    break;

                case 3:
                    // Book a ticket
                    System.out.print("Enter Passenger Name: ");
                    String passengerName = scanner.nextLine();
                    System.out.print("Enter Passenger Age: ");
                    int age = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Seat Type (e.g., Window/Aisle): ");
                    String seatType = scanner.nextLine();
                    System.out.print("Enter Route for Booking (e.g., A-B): ");
                    String bookingRoute = scanner.nextLine();
                    system.bookTicket(passengerName, age, seatType, bookingRoute);
                    break;

                case 4:
                    // Cancel a ticket
                    System.out.print("Enter Passenger Name: ");
                    String cancelName = scanner.nextLine();
                    System.out.print("Enter Route for Cancellation (e.g., A-B): ");
                    String cancelRoute = scanner.nextLine();
                    system.cancelTicket(cancelName, cancelRoute);
                    break;

                case 5:
                    // Display bookings
                    system.displayBookings();
                    break;

                case 6:
                    // Display available seats
                    System.out.print("Enter Route to Check Seats (e.g., A-B): ");
                    String seatRoute = scanner.nextLine();
                    system.displayAvailableSeats(seatRoute);
                    break;

                case 7:
                    // Exit the program
                    System.out.println("Exiting the system. Goodbye!");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}
