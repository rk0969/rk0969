#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure to represent an appointment
struct Appointment {
    char patientName[50];
    char date[11];  // Assuming date format: YYYY-MM-DD
    char time[10];  // Assuming time format: HH:MM AM/PM
};

// Structure to represent the appointment scheduler
struct AppointmentScheduler {
    struct Appointment appointments[100];  // Assuming a maximum of 100 appointments
    int size;
};

// Function to schedule a new appointment
void scheduleAppointment(struct AppointmentScheduler *scheduler, const char *patientName, const char *date, const char *time) {
    struct Appointment newAppointment;
    strncpy(newAppointment.patientName, patientName, sizeof(newAppointment.patientName));
    strncpy(newAppointment.date, date, sizeof(newAppointment.date));
    strncpy(newAppointment.time, time, sizeof(newAppointment.time));

    scheduler->appointments[scheduler->size] = newAppointment;
    scheduler->size++;

    printf("Appointment scheduled for %s on %s at %s\n", patientName, date, time);
}

// Function to view the next scheduled appointment
void viewNextAppointment(const struct AppointmentScheduler *scheduler) {
    if (scheduler->size > 0) {
        struct Appointment nextAppointment = scheduler->appointments[0];
        printf("Next appointment: %s on %s at %s\n", nextAppointment.patientName, nextAppointment.date, nextAppointment.time);
    } else {
        printf("No appointments scheduled.\n");
    }
}

// Function to cancel the next scheduled appointment
void cancelNextAppointment(struct AppointmentScheduler *scheduler) {
    if (scheduler->size > 0) {
        struct Appointment canceledAppointment = scheduler->appointments[0];
        for (int i = 1; i < scheduler->size; i++) {
            scheduler->appointments[i - 1] = scheduler->appointments[i];
        }
        scheduler->size--;

        printf("Appointment canceled for %s on %s at %s\n", canceledAppointment.patientName, canceledAppointment.date, canceledAppointment.time);
    } else {
        printf("No appointments to cancel.\n");
    }
}

int main() {
    struct AppointmentScheduler scheduler;
    scheduler.size = 0;

    scheduleAppointment(&scheduler, "John Doe", "2023-11-20", "10:00 AM");
    scheduleAppointment(&scheduler, "Jane Smith", "2023-11-21", "02:30 PM");

    viewNextAppointment(&scheduler);

    cancelNextAppointment(&scheduler);

    viewNextAppointment(&scheduler);

    return 0;
}
