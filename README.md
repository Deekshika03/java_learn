# this is for FDs RDs interest calculation

import java.util.Scanner;

// Base class
class Deposit {
    String accountId;
    String accountHolderName;
    double principalAmount;
    int tenure; // In years for FD, in months for RD

    void getDetails(Scanner sc) {
        System.out.print("Enter Account ID: ");
        accountId = sc.nextLine();

        System.out.print("Enter Account Holder Name: ");
        accountHolderName = sc.nextLine();

        System.out.print("Enter Principal Amount: ");
        principalAmount = sc.nextDouble();

        System.out.print("Enter Tenure (years for FD, months for RD): ");
        tenure = sc.nextInt();
        sc.nextLine(); // consume leftover newline
    }

    void displayDetails() {
        System.out.println("Account ID: " + accountId);
        System.out.println("Account Holder Name: " + accountHolderName);
        System.out.println("Principal Amount: " + principalAmount);
        System.out.println("Tenure: " + tenure);
    }
}

// Derived class for Fixed Deposit
class FD extends Deposit {
    double rateOfInterestFD;

    double calculateInterestFD() {
        // Compound Interest: A = P * (1 + R/100)^T
        double amount = principalAmount * Math.pow((1 + rateOfInterestFD / 100), tenure);
        return amount - principalAmount; // Interest only
    }
}

// Derived class for Recurring Deposit
class RD extends Deposit {
    double rateOfInterestRD;

    double calculateInterestRD() {
        double N = tenure; // in months
        // Formula: P × (N × (N+1)/2) × (R/100) / 12
        return principalAmount * (N * (N + 1) / 2.0) * (rateOfInterestRD / 100) / 12.0;
    }
}

// Main class
public class DepositCalculator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("Select Deposit Type:\n1. Fixed Deposit (FD)\n2. Recurring Deposit (RD)");
        int choice = sc.nextInt();
        sc.nextLine(); // consume newline

        if (choice == 1) {
            FD fd = new FD();
            fd.getDetails(sc);

            System.out.print("Enter Rate of Interest for FD: ");
            fd.rateOfInterestFD = sc.nextDouble();

            double interest = fd.calculateInterestFD();

            System.out.println("\n--- FD Account Details ---");
            fd.displayDetails();
            System.out.printf("Interest Earned: %.2f\n", interest);
            System.out.printf("Total Amount after Interest: %.2f\n", fd.principalAmount + interest);

        } else if (choice == 2) {
            RD rd = new RD();
            rd.getDetails(sc);

            System.out.print("Enter Rate of Interest for RD: ");
            rd.rateOfInterestRD = sc.nextDouble();

            double interest = rd.calculateInterestRD();

            System.out.println("\n--- RD Account Details ---");
            rd.displayDetails();
            System.out.printf("Total Interest Earned: %.2f\n", interest);
            System.out.printf("Total Amount after Interest: %.2f\n", rd.principalAmount + interest);

        } else {
            System.out.println("Invalid choice!");
        }

        sc.close();
    }
}
