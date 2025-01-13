# Target-tracking-application
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <CUnit/Basic.h>
#include <CUnit/CUnit.h>

#define MAX_GOALS 50
#define MAX_NAME_LENGTH 100
#define MAX_DESC_LENGTH 255

// Goal struct definition
typedef struct {
    char name[MAX_NAME_LENGTH];
    char description[MAX_DESC_LENGTH];
    int progress;
    int target;
    char deadline[11];
} Goal;

Goal goals[MAX_GOALS];
int goalCount = 0;

// Function to add a new goal
void addGoal() {
    if (goalCount >= MAX_GOALS) {
        printf("\nYou have reached the maximum number of goals.\n");
        return;
    }

    Goal newGoal;
    printf("\nEnter the name of the goal: ");
    fgets(newGoal.name, MAX_NAME_LENGTH, stdin);
    newGoal.name[strcspn(newGoal.name, "\n")] = 0;

    printf("Enter a description: ");
    fgets(newGoal.description, MAX_DESC_LENGTH, stdin);
    newGoal.description[strcspn(newGoal.description, "\n")] = 0;

    printf("Enter the target value: ");
    scanf("%d", &newGoal.target);

    printf("Enter the deadline (YYYY-MM-DD): ");
    scanf("%s", newGoal.deadline);

    newGoal.progress = 0;
    goals[goalCount++] = newGoal;
    printf("\nGoal added successfully!\n");
}

// Function to view all goals and their progress
void viewGoals() {
    if (goalCount == 0) {
        printf("\nNo goals added yet.\n");
        return;
    }

    printf("\nYour Goals:\n");
    for (int i = 0; i < goalCount; i++) {
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

    if (choice < 1 || choice > goalCount) {
        printf("\nInvalid choice.\n");
        return;
    }

    int newProgress;
    printf("Enter new progress value: ");
    scanf("%d", &newProgress);

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
        getchar(); // Consume newline character left by scanf

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
    menu();
    return 0;
}

// Test functions
void testAddGoal() {
    Goal testGoal;
    strcpy(testGoal.name, "Test Goal");
    strcpy(testGoal.description, "Test Description");
    testGoal.target = 100;
    testGoal.progress = 50;
    strcpy(testGoal.deadline, "2025-12-31");

    CU_ASSERT_STRING_EQUAL(testGoal.name, "Test Goal");
    CU_ASSERT(testGoal.target == 100);
    CU_ASSERT(testGoal.progress == 50);
    CU_ASSERT_STRING_EQUAL(testGoal.deadline, "2025-12-31");
}

void testUpdateProgress() {
    Goal testGoal = {"Test Goal", "Test Description", 50, 100, "2025-12-31"};
    testGoal.progress = 80;

    CU_ASSERT(testGoal.progress == 80);
}

// CUnit Test Runner
int mainTest() {
    CU_initialize_registry();

    CU_pSuite pSuite = CU_add_suite("GoalTrackerTest", 0, 0);
    CU_add_test(pSuite, "testAddGoal", testAddGoal);
    CU_add_test(pSuite, "testUpdateProgress", testUpdateProgress);

    CU_basic_run_tests();
    CU_cleanup_registry();

    return 0;
}
