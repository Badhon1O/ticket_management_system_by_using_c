# ticket-_management_system_using_c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <windows.h>

#define MAX_SEATS 100

typedef struct
{
    char name[50];
    char departure_time[100];
    int total_seats;
    int avail_seats;
    int total_tickets_e;
    int avail_tickets_e;
    int total_tickets_b;
    int avail_tickets_b;
    char start_time[100];
    char end_time[100];
    int e_seats[MAX_SEATS];
    int b_seats[MAX_SEATS];
} Train;

void display_menu();
void view_trains(Train trains[], int num_trains);
void book_ticket(Train trains[], int num_trains);
int pre_payment(float amount);
void goback_exit();
void exit_project();

int main()
{
    Train trains[] =
    {
        {"Padma Express", "7:00 AM", 200, 200, 100, 100, 50, 50, "Dhaka- 7:00AM", "Rajshahi- 5:00PM", {0}, {0}},
        {"Silk City Express", "3:00 PM", 240, 240, 120, 120, 60, 60, "Dhaka- 3:00PM", "Rajshahi- 12:00AM", {0}, {0}},
        {"Dhumketu Express", "6:00 PM", 160, 160, 80, 80, 40, 40, "Dhaka- 6:00PM", "Rajshahi- 3:00AM", {0}, {0}},
        {"Sundarban Express", "8:00 AM", 200, 200, 100, 100, 50, 50, "Dhaka- 8:00AM", "Rajshahi- 6:00PM", {0}, {0}}
    };

    int num_trains = 4;

    FILE *fp = fopen("train_details.txt", "w");

    if (fp == NULL)
    {
        printf("Error opening file!\n");
        return 1;
    }

    fprintf(fp, "Train Details:\n");

    for (int i = 0; i < num_trains; i++)
    {
        fprintf(fp, "Train Name: %s\n", trains[i].name);
        fprintf(fp, "Departure Time: %s\n", trains[i].departure_time);
        fprintf(fp, "Total Seats: %d\n", trains[i].total_seats);
        fprintf(fp, "Available Seats: %d\n", trains[i].avail_seats);
        fprintf(fp, "Total Economy Tickets: %d\n", trains[i].total_tickets_e);
        fprintf(fp, "Available Economy Tickets: %d\n", trains[i].avail_tickets_e);
        fprintf(fp, "Total Business Tickets: %d\n", trains[i].total_tickets_b);
        fprintf(fp, "Available Business Tickets: %d\n", trains[i].avail_tickets_b);
        fprintf(fp, "Start Time: %s and End Time: %s\n", trains[i].start_time, trains[i].end_time);
        fprintf(fp, "\n");
    }

    fclose(fp);

    int choice;

    do
    {
        display_menu();

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            view_trains(trains, num_trains);
            break;
        case 2:

            book_ticket(trains, num_trains);
            goback_exit();
            break;
        case 3:
            exit_project();
            break;
        default:
            system("cls");
            printf("\nInvalid choice! Please try again.\n\n");
            goback_exit();
        }
    }
    while (choice != 3);

    return 0;
}

void display_menu()
{
    printf("\n====== Ticket Management System ======\n");
    printf("1. View available trains\n");
    printf("2. Book a ticket\n");
    printf("3. Exit\n");
    printf("======================================\n");
}

void view_trains(Train trains[], int num_trains)
{
    printf("\nAvailable Trains:\n");

    for (int i = 0; i < num_trains; i++)
    {
        printf("%d. Train Name: %s\n", i+1, trains[i].name);
        printf("   Departure Time: %s\n", trains[i].departure_time);
        printf("   Available Economy Seats: %d\n", trains[i].avail_seats - trains[i].total_tickets_e);
        printf("   Available Business Seats: %d\n", trains[i].avail_seats - trains[i].total_tickets_b);
        printf("   Available Economy Tickets: %d\n", trains[i].avail_tickets_e);
        printf("   Available Business Tickets: %d\n", trains[i].avail_tickets_b);
        printf("   Start Time: %s, End Time: %s\n", trains[i].start_time, trains[i].end_time);
        printf("\n");
    }
}

void book_ticket(Train trains[], int num_trains)
{
    int choice, class_choice, seat_choice;

    int train_num = 0;

    while(!train_num)
    {
        printf("Enter the train number to book a ticket: ");
        scanf("%d", &choice);

        choice--; // Adjust for 0-based indexing

        if (choice < 0 || choice >= num_trains)
        {
            printf("No train found! Please try again.\n");
            train_num = 0;
        }


        else
        {
            system("cls");
            // Display selected train information
            printf("\nSelected Train Information:\n\n");
            printf("Train Name: %s\n", trains[choice].name);
            printf("Departure Time: %s\n", trains[choice].departure_time);
            printf("Available Economy Seats: %d\n", trains[choice].avail_seats - trains[choice].total_tickets_e);
            printf("Available Business Seats: %d\n", trains[choice].avail_seats - trains[choice].total_tickets_b);
            printf("Available Economy Tickets: %d\n", trains[choice].avail_tickets_e);
            printf("Available Business Tickets: %d\n", trains[choice].avail_tickets_b);
            printf("Start Time: %s, End Time: %s\n", trains[choice].start_time, trains[choice].end_time);

            int ticket_class = 0;

            while(!ticket_class)
            {

                printf("\nEnter ticket class (1 for Economy, 2 for Business): ");
                scanf("%d", &class_choice);

                if (class_choice != 1 && class_choice != 2)
                {
                    printf("Invalid class choice!\n");
                    ticket_class = 0;
                }
                else
                {
                    ticket_class = 1;
                }
            }

            if (class_choice == 1)
            {
                if (trains[choice].avail_tickets_e > 0 && trains[choice].avail_seats - trains[choice].total_tickets_e > 0)
                {
                    printf("Available Economy Seats: ");
                    for (int i = 0; i < trains[choice].total_seats; i++)
                    {
                        if (trains[choice].e_seats[i] == 0)
                        {
                            printf("%d ", i + 1);
                        }
                    }

                    int seat_num1 = 0;

                    while(!seat_num1)
                    {
                        printf("\n\nEnter seat number: ");
                        scanf("%d", &seat_choice);
                        if (seat_choice < 1 || seat_choice > trains[choice].total_seats || trains[choice].e_seats[seat_choice - 1] == 1)
                        {
                            printf("Invalid seat choice!\n");
                            seat_num1 = 0;
                        }
                        else
                        {
                            seat_num1 = 1;
                        }
                    }

                    trains[choice].e_seats[seat_choice - 1] = 1;
                    trains[choice].avail_tickets_e--;
                    trains[choice].total_tickets_e++;
                    printf("Economy ticket booked successfully for seat %d!\n", seat_choice);
                    float price = 400.0; // Assuming ticket price is $50 for economy class
                    int payment_status = pre_payment(price);
                    if (payment_status)
                    {
                        printf("Payment received successfully!\n");
                    }
                    else
                    {
                        printf("Payment failed!\n");
                        printf("Ticket booking canceled.\n");
                        // Reset the booked seat and ticket count
                        trains[choice].e_seats[seat_choice - 1] = 0;
                        trains[choice].avail_tickets_e++;
                        trains[choice].total_tickets_e--;
                    }
                }

                else
                {
                    printf("No available Economy tickets for this train!\n");
                }
            }
            else
            {
                if (trains[choice].avail_tickets_b > 0 && trains[choice].avail_seats - trains[choice].total_tickets_b > 0)
                {
                    printf("Available Business Seats: ");
                    for (int i = 0; i < trains[choice].total_seats; i++)
                    {
                        if (trains[choice].b_seats[i] == 0)
                        {
                            printf("%d ", i + 1);
                        }
                    }

                    int seat_num2 = 0;

                    while(!seat_num2)
                    {
                        printf("\nEnter seat number: ");
                        scanf("%d", &seat_choice);
                        if (seat_choice < 1 || seat_choice > trains[choice].total_seats || trains[choice].b_seats[seat_choice - 1] == 1)
                        {
                            printf("Invalid seat choice!\n");
                            seat_num2 = 0;
                        }

                        else
                        {
                            seat_num2 = 1;
                        }
                    }

                    trains[choice].b_seats[seat_choice - 1] = 1;
                    trains[choice].avail_tickets_b--;
                    trains[choice].total_tickets_b++;
                    printf("Business ticket booked successfully for seat %d!\n", seat_choice);
                    float price = 750.0; // Assuming ticket price is $100 for business class
                    int payment_status = pre_payment(price);
                    if (payment_status)
                    {
                        printf("Payment received successfully!\n");
                    }
                    else
                    {
                        printf("Payment failed!\n");
                        printf("Ticket booking canceled.\n");
                        // Reset the booked seat and ticket count
                        trains[choice].b_seats[seat_choice - 1] = 0;
                        trains[choice].avail_tickets_b++;
                        trains[choice].total_tickets_b--;
                    }
                }
                else
                {
                    printf("No available Business tickets for this train!\n");
                }
            }

            train_num = 1;
        }
    }
}

int pre_payment(float amount)
{
    float pay_amount;
    printf("Total ticket price: %.2fBDT\n", amount);
    printf("Enter payment amount: ");
    scanf("%f", &pay_amount);
    if (pay_amount < amount)
    {
        printf("Insufficient payment amount!\n");
        return 0; // Payment failed
    }
    printf("Payment received: %.2fBDT\n", pay_amount);
    printf("Change: %.2fBDT\n", pay_amount - amount);
    return 1; // Payment successful
}

void goback_exit()
{
    getchar();
    char Option;
    printf(" Go back(b)? or Exit(0)?: ");
    scanf("%c",&Option);
    if(Option == '0')
    {
        exit_project();
    }
    else
    {
        system("cls");
    }
}

void exit_project()
{
    system("cls");
    int i;
    char ThankYou[100] = "Thank You for using this system. Have a good Day!\n";
    for(i=0; i<strlen(ThankYou); i++)
    {
        printf("%c",ThankYou[i]);
        Sleep(40);
    }
    exit(0);
}
