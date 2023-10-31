#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <limits.h>

#define MAX_PROCESSES 100
#define MAX_ARRIVAL_TIME 10
#define MAX_BURST_TIME 20

struct Process {
    char name[10];
    int arrival_time;
    int burst_time;
};

// Function prototypes
void roundRobin(struct Process processes[], int num_processes, int time_quantum, int max_time_units);
void sjf(struct Process processes[], int num_processes, int max_time_units);
void sortProcessesByArrivalTime(struct Process processes[], int num_processes);

int main() {
    srand(time(NULL));

    int num_processes;
    printf("Enter the number of processes: ");
    scanf("%d", &num_processes);

    if (num_processes <= 0 || num_processes > MAX_PROCESSES) {
        printf("Invalid number of processes.\n");
        return 1;
    }

    struct Process processes[MAX_PROCESSES];

    for (int i = 0; i < MAX_PROCESSES; i++) {
        strcpy(processes[i].name, "");
        processes[i].arrival_time = 0;
        processes[i].burst_time = 0;
    }

    int used_arrival_times[MAX_ARRIVAL_TIME + 1] = {0};

    for (int i = 0; i < num_processes; i++) {
        snprintf(processes[i].name, sizeof(processes[i].name), "P%d", i + 1);

        int arrival_time;
        do {
            arrival_time = rand() % (MAX_ARRIVAL_TIME + 1);
        } while (used_arrival_times[arrival_time] == 1);
        used_arrival_times[arrival_time] = 1;

        processes[i].arrival_time = arrival_time;
        processes[i].burst_time = (rand() % MAX_BURST_TIME) + 1;
    }

    printf("\nProcess Details:\n");
    for (int i = 0; i < num_processes; i++) {
        printf("%s (Arrival Time: %d, Burst Time: %d)\n", processes[i].name, processes[i].arrival_time, processes[i].burst_time);
    }

    int time_quantum;
    printf("Enter the time quantum: ");
    scanf("%d", &time_quantum);

    int max_time_units = 100;

    sortProcessesByArrivalTime(processes, num_processes);

    printf("\nRound Robin Schedule:\n");
    roundRobin(processes, num_processes, time_quantum, max_time_units);

    printf("\nSJF Schedule:\n");
    sjf(processes, num_processes, max_time_units);

    return 0;
}

void roundRobin(struct Process processes[], int num_processes, int time_quantum, int max_time_units) {
    int remaining_time[num_processes];
    int turnaround_time[num_processes];
    int waiting_time[num_processes];
    int finish_time[num_processes];
    int current_time = 0;
    int completed_processes = 0;

    for (int i = 0; i < num_processes; i++) {
        remaining_time[i] = processes[i].burst_time;
        finish_time[i] = -1;
    }

    while (completed_processes < num_processes && current_time < max_time_units) {
        for (int i = 0; i < num_processes; i++) {
            if (remaining_time[i] > 0) {
                if (remaining_time[i] <= time_quantum) {
                    current_time += remaining_time[i];
                    finish_time[i] = current_time;
                    remaining_time[i] = 0;
                    completed_processes++;
                } else {
                    current_time += time_quantum;
                    remaining_time[i] -= time_quantum;
                }
            }
        }
    }

    for (int i = 0; i < num_processes; i++) {
        turnaround_time[i] = finish_time[i] - processes[i].arrival_time;
        waiting_time[i] = turnaround_time[i] - processes[i].burst_time;
    }

    printf("\nProcess-wise Finish, Waiting, and Turnaround Times (Round Robin):\n");
    for (int i = 0; i < num_processes; i++) {
        printf("%s - Finish Time: %d, Waiting Time: %d, Turnaround Time: %d\n", processes[i].name, finish_time[i], waiting_time[i], turnaround_time[i]);
    }

    float total_waiting_time = 0;
    for (int i = 0; i < num_processes; i++) {
        total_waiting_time += waiting_time[i];
    }

    float avg_turnaround_time = 0;
    for (int i = 0; i < num_processes; i++) {
        avg_turnaround_time += turnaround_time[i];
    }
    avg_turnaround_time /= num_processes;

    printf("\nAverage Waiting Time (Round Robin): %.2f\n", total_waiting_time / num_processes);
    printf("Average Turnaround Time (Round Robin): %.2f\n", avg_turnaround_time);
}


void sjf(struct Process processes[], int num_processes, int max_time_units) {
    int remaining_time[num_processes];
    int turnaround_time[num_processes];
    int waiting_time[num_processes];
    int current_time = 0;
    int completed_processes = 0;

    for (int i = 0; i < num_processes; i++) {
        remaining_time[i] = processes[i].burst_time;
    }

    while (completed_processes < num_processes && current_time < max_time_units) {
        int min_burst_time = INT_MAX;
        int shortest_job_index = -1;

        for (int i = 0; i < num_processes; i++) {
            if (remaining_time[i] > 0 && processes[i].arrival_time <= current_time && remaining_time[i] < min_burst_time) {
                min_burst_time = remaining_time[i];
                shortest_job_index = i;
            }
        }

        if (shortest_job_index == -1) {
            current_time++;
        } else {
            current_time += remaining_time[shortest_job_index];
            turnaround_time[shortest_job_index] = current_time - processes[shortest_job_index].arrival_time;
            remaining_time[shortest_job_index] = 0;
            completed_processes++;
        }
    }

    for (int i = 0; i < num_processes; i++) {
        waiting_time[i] = turnaround_time[i] - processes[i].burst_time;
    }

    printf("\nProcess-wise Waiting and Turnaround Times (SJF):\n");
    for (int i = 0; i < num_processes; i++) {
        printf("%s - Waiting Time: %d, Turnaround Time: %d\n", processes[i].name, waiting_time[i], turnaround_time[i]);
    }

    float total_waiting_time = 0;
    for (int i = 0; i < num_processes; i++) {
        total_waiting_time += waiting_time[i];
    }

    float avg_turnaround_time = 0;
    for (int i = 0; i < num_processes; i++) {
        avg_turnaround_time += turnaround_time[i];
    }
    avg_turnaround_time /= num_processes;

    printf("\nAverage Waiting Time (SJF): %.2f\n", total_waiting_time / num_processes);
    printf("Average Turnaround Time (SJF): %.2f\n", avg_turnaround_time);
}

void sortProcessesByArrivalTime(struct Process processes[], int num_processes) {
    for (int i = 0; i < num_processes - 1; i++) {
        for (int j = i + 1; j < num_processes; j++) {
            if (processes[i].arrival_time > processes[j].arrival_time) {
                struct Process temp = processes[i];
                processes[i] = processes[j];
                processes[j] = temp;
            }
        }
    }
}
