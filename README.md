#include <stdio.h>
#include <string.h>

#define MAX_GOALS 100
#define MAX_NAME_LENGTH 100
#define MAX_DESC_LENGTH 1000

// Goal struct definition
typedef struct {
    char name[MAX_NAME_LENGTH]; // Name of the goal
    char description[MAX_DESC_LENGTH]; // A detailed description of the goal
    int progress; // Current progress toward the goal
    int target; // The target value to reach
    char deadline[11]; // Deadline in format YYYY-MM-DD
} Goal;

// Array to store goals
Goal goals[MAX_GOALS];
int goalCount = 0; // Counter for tracking the number of goals

// Function to add a new goal
void addGoal() {
    if (goalCount >= MAX_GOALS) {
        printf("\nYou have reached the maximum number of goals.\n");
        return;
    }

    Goal newGoal;

    printf("\nEnter the name of the goal:\n");
    fgets(newGoal.name, sizeof(newGoal.name), stdin);
    newGoal.name[strcspn(newGoal.name, "\n")] = 0; // Remove newline character

    printf("Enter a description:\n");
    fgets(newGoal.description, sizeof(newGoal.description), stdin);
    newGoal.description[strcspn(newGoal.description, "\n")] = 0; // Remove newline character

    printf("Enter the target value:\n");
    scanf("%d", &newGoal.target);
    getchar(); // Consume leftover newline character

    printf("Enter the deadline (YYYY-MM-DD):\n");
    fgets(newGoal.deadline, sizeof(newGoal.deadline), stdin);
    newGoal.deadline[strcspn(newGoal.deadline, "\n")] = 0; // Remove newline character

    newGoal.progress = 0; // Initialize progress to 0
    goals[goalCount++] = newGoal; // Add the new goal to the array

    printf("\nGoal added successfully!\n");
}

// Function to view all goals and their progress
void viewGoals() {
    if (goalCount == 0) {
        printf("\nNo goals added yet.\n");
        return;
    }

    printf("\nYour Goals:\n");
    int i;
    for ( i = 0; i < goalCount; i++) {
        printf("%d. %s\n", i + 1, goals[i].name);
        printf("   Description: %s\n", goals[i].description);
        printf("   Progress: %d/%d\n", goals[i].progress, goals[i].target);
        printf("   Deadline: %s\n", goals[i].deadline);
    }
}

// Function to update progress on a goal
void updateProgress() {
    if (goalCount == 0) {
        printf("\nNo goals to update.\n");
        return;
    }

    int choice;
    printf("\nEnter the goal number to update: ");
    scanf("%d", &choice);
    getchar(); // Consume leftover newline character

    if (choice < 1 || choice > goalCount) {
        printf("\nInvalid choice.\n");
        return;
    }

    int newProgress;
    printf("Enter new progress value: ");
    scanf("%d", &newProgress);
    getchar(); // Consume leftover newline character

    if (newProgress < 0 || newProgress > goals[choice - 1].target) {
        printf("\nInvalid progress value.\n");
        return;
    }

    goals[choice - 1].progress = newProgress;
    printf("\nProgress updated successfully!\n");
}

// Main menu function
void menu() {
    int choice;
    while (1) {
        printf("\n--- Target Tracking App ---\n");
        printf("1. Add a Goal\n");
        printf("2. View Goals\n");
        printf("3. Update Progress\n");
        printf("4. Exit\n");
        printf("Choose an option: ");
        scanf("%d", &choice);
        getchar(); // Consume leftover newline character

        switch (choice) {
            case 1:
                addGoal();
                break;
            case 2:
                viewGoals();
                break;
            case 3:
                updateProgress();
                break;
            case 4:
                printf("\nGoodbye!\n");
                return;
            default:
                printf("\nInvalid choice. Please try again.\n");
        }
    }
}

// Main function to start the application
int main() {
    menu(); // Start the menu function
    return 0;
}

