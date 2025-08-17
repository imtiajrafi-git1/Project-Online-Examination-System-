// PROJECT NAME: ONLINE EXAMINATION SYSTEM
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 50

int main()
{

    system("color 79");
    int choice;
    char username[MAX], password[MAX];
    FILE *fp;

    while (1)
    {
        printf("\n===== LOGIN OPTIONS =====\n");
        printf("1. Teacher Login\n");
        printf("2. Student Register\n");
        printf("3. Student Login\n");
        printf("4. Exit\n");
        printf("=========================\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();

        switch (choice)
        {

        // ===== TEACHER LOGIN PART =====
        case 1:
            system("cls");
            printf("\n--- Teacher Login ---\n");
            printf("Username: ");
            fgets(username, MAX, stdin);
            username[strcspn(username, "\n")] = '\0';

            printf("Password: ");
            fgets(password, MAX, stdin);
            password[strcspn(password, "\n")] = '\0';


            if (strcmp(username, "diu teachers") == 0 && strcmp(password, "diu1234") == 0)
            {
                printf(" Teacher login successful.\n");
                printf(" Welcome, %s!\n", username);

                int teacher_choice;
                while (1)
                {
                    printf("\n--- Teacher Panel ---\n");
                    printf("1. Create Exam\n");
                    printf("2. View Questions\n");
                    printf("3. View Results\n");
                    printf("4. View Feedback\n");
                    printf("5. Logout\n");
                    printf("Enter your choice: ");
                    scanf("%d", &teacher_choice);
                    getchar();

                    switch (teacher_choice)
                    {
                    case 1:

                    {
                        // ==== CODE FOR CREATE EXAM ====
                        system("cls");
                        printf("\n--- CREATE EXAM ---\n");
                        int num_questions;
                        printf("How many questions to add? ");
                        scanf("%d", &num_questions);
                        getchar();

                        FILE *exam_fp = fopen("exam.txt", "w");
                        if (exam_fp == NULL)
                        {
                            printf(" Cannot create exam file.\n");
                            break;
                        }

                        char question[200], option_a[100], option_b[100], option_c[100], option_d[100], correct_option[2];
                        for (int i = 1; i <= num_questions; i++)
                        {
                            printf("\nQuestion %d: ", i);
                            fgets(question, sizeof(question), stdin);
                            question[strcspn(question, "\n")] = '\0';

                            printf("Option a: ");
                            fgets(option_a, sizeof(option_a), stdin);
                            option_a[strcspn(option_a, "\n")] = '\0';

                            printf("Option b: ");
                            fgets(option_b, sizeof(option_b), stdin);
                            option_b[strcspn(option_b, "\n")] = '\0';

                            printf("Option c: ");
                            fgets(option_c, sizeof(option_c), stdin);
                            option_c[strcspn(option_c, "\n")] = '\0';

                            printf("Option d: ");
                            fgets(option_d, sizeof(option_d), stdin);
                            option_d[strcspn(option_d, "\n")] = '\0';

                            printf("Correct option (a/b/c/d): ");
                            fgets(correct_option, sizeof(correct_option), stdin); // Read only one character
                            getchar(); // Clear input buffer

                            fprintf(exam_fp, "%s\n%s\n%s\n%s\n%s\n%c\n", question, option_a, option_b, option_c, option_d, correct_option[0]);
                        }
                        fclose(exam_fp);
                        printf(" Exam created successfully!\n");
                        break;
                    }
                    case 2:

                        // ==== CODE FOR VIEWING THE QUESTION =====
                        printf(" View The Created Questions\n");
                        FILE *exam_fp = fopen("exam.txt", "r");
                        if (exam_fp == NULL)
                        {
                            printf(" No exam questions found.\n");
                            break;
                        }
                        char lines[256];
                        while (fgets(lines, sizeof(lines), exam_fp))
                        {
                            printf("%s", lines);
                        }
                        break;
                    case 3:
                        printf(" View THe Results\n");
                        system("cls");
                        printf("\n--- VIEW RESULTS ---\n");

                        FILE *res_fp = fopen("results.txt", "r");
                        if (res_fp == NULL)
                        {
                            printf(" No results available.\n");
                            break;
                        }

                        char line[256];
                        int found_result = 0;

                        while (fgets(line, sizeof(line), res_fp))
                        {
                            printf("%s", line);
                            found_result = 1;
                        }

                        fclose(res_fp);

                        if (!found_result)
                        {
                            printf(" No results found.\n");
                        }
                        break;
                    case 4:
                        printf(" View The Feedback\n");
                        system("cls");
                        printf("\n--- VIEW FEEDBACK ---\n");
                        FILE *feedback_fp = fopen("feedback.txt", "r");
                        if (feedback_fp == NULL)
                        {
                            printf(" No feedback found.\n");
                            break;
                        }
                        char feedback_line[500];
                        while (fgets(feedback_line, sizeof(feedback_line), feedback_fp))
                        {
                            printf("%s", feedback_line);
                        }
                        fclose(feedback_fp);
                        break;

                    // ...code for existing...
                    case 5:
                        printf(" Logging out...\n");
                        goto end_teacher;

                    default:
                        printf(" Invalid choice. Try again.\n");
                    }
                }


            }


        // ===== STUDENT REGISTER =====
        case 2:

            fp = fopen("students.txt", "a");
            if (fp == NULL)
            {
                printf(" File does not opened yet.\n");
                break;
            }

            system("cls");
            printf("\n--- Student Registration ---\n");
            printf("Choose a username: ");
            fgets(username, MAX, stdin);
            username[strcspn(username, "\n")] = '\0';

            printf("Choose a password: ");
            fgets(password, MAX, stdin);
            password[strcspn(password, "\n")] = '\0';

            fprintf(fp, "%s %s\n", username, password);
            fclose(fp);
            printf(" Student registered successfully.\n");
            break;


        // ===== STUDENT LOGIN =====
        case 3:

            fp = fopen("students.txt", "r");
            if (fp == NULL)
            {
                printf(" File does not exist.\n");
                break;
            }

            char file_user[MAX], file_pass[MAX];
            int found = 0;

            system("cls");
            printf("\n--- Student Login ---\n");
            printf("Username: ");
            fgets(username, MAX, stdin);
            username[strcspn(username, "\n")] = '\0';

            printf("Password: ");
            fgets(password, MAX, stdin);
            password[strcspn(password, "\n")] = '\0';

            while (fscanf(fp, "%s %s", file_user, file_pass) != EOF)
            {
                if (strcmp(username, file_user) == 0 && strcmp(password, file_pass) == 0)
                {
                    found = 1;
                    break;
                }
            }
            fclose(fp);

            if (found)
            {
                printf(" Student login successful.\n");
                printf(" Welcome, %s!\n", username);

                int student_choice;
                while (1)
                {
                    printf("\n--- Student Panel ---\n");
                    printf("1. Take Exam\n");
                    printf("2. View Results\n");
                    printf("3. Feedback\n");
                    printf("4. Logout\n");
                    printf("Enter your choice: ");
                    scanf("%d", &student_choice);
                    getchar();

                    switch (student_choice)
                    {

                    // ===== TAKE EXAM CODE FOR STUDENT =====
                    case 1:

                    {
                        system("cls");
                        printf("\n--- TAKE EXAM ---\n");
                        FILE *exam_fp = fopen("exam.txt", "r");
                        if (exam_fp == NULL)
                        {
                            printf(" No exam available.\n");
                            break;
                        }
                        char question[200], option_a[100], option_b[100], option_c[100], option_d[100], correct;
                        int question_count = 0, result = 0;
                        while (fgets(question, sizeof(question), exam_fp))
                        {
                            question[strcspn(question, "\n")] = '\0';
                            fgets(option_a, sizeof(option_a), exam_fp);
                            option_a[strcspn(option_a, "\n")] = '\0';
                            fgets(option_b, sizeof(option_b), exam_fp);
                            option_b[strcspn(option_b, "\n")] = '\0';
                            fgets(option_c, sizeof(option_c), exam_fp);
                            option_c[strcspn(option_c, "\n")] = '\0';
                            fgets(option_d, sizeof(option_d), exam_fp);
                            option_d[strcspn(option_d, "\n")] = '\0';
                            fgets(&correct, 2, exam_fp); // Read the correct answer as a single character
                            while (fgetc(exam_fp) != '\n' && !feof(exam_fp)); // Skip to next line if needed
                            question_count++;
                            printf("\nQuestion %d: %s\n", question_count, question);
                            printf("a. %s\n", option_a);
                            printf("b. %s\n", option_b);
                            printf("c. %s\n", option_c);
                            printf("d. %s\n", option_d);
                            char answer;
                            printf("Your answer (a/b/c/d): ");
                            scanf(" %c", &answer);
                            getchar();
                            if (answer == correct)
                            {
                                printf(" Correct answer!\n");
                                result++;
                            }
                            else
                            {
                                printf(" Incorrect answer. The correct answer was %c.\n", correct);
                            }
                        }
                        fclose(exam_fp);
                        FILE *res_fp = fopen("results.txt", "a");
                        if (res_fp == NULL)
                        {
                            printf(" Cannot open results file.\n");
                            break;
                        }
                        fprintf(res_fp, "User: %s, Score: %d/%d\n", username, result, question_count);
                        fclose(res_fp);
                        printf(" Exam completed. Your score has been recorded.\n");
                        break;
                    }


                    // ===== VIEW STUDENTS RESULTS =====
                    case 2:

                    {
                        system("cls");
                        printf("\n--- VIEW RESULTS ---\n");

                        FILE *res_fp = fopen("results.txt", "r");
                        if (res_fp == NULL)
                        {
                            printf(" No results available.\n");
                            break;
                        }

                        char line[100];
                        int found_result = 0;

                        while (fgets(line, sizeof(line), res_fp))
                        {
                            if (strstr(line, username))
                            {
                                printf("%s", line);
                                found_result = 1;
                            }
                        }

                        fclose(res_fp);

                        if (!found_result)
                        {
                            printf(" No result found for user: %s\n", username);
                        }
                        break;
                    }

                    // ===== FEEDBACK =====
                    case 3:

                        // ==== Code for feedback functionality goes here ====
                        printf(" Feedback \n");
                        FILE *feedback_fp = fopen("feedback.txt", "a");
                        if (feedback_fp == NULL)
                        {
                            printf(" Cannot open feedback file.\n");
                            break;
                        }
                        char feedback[MAX];
                        printf("Enter your feedback: ");
                        fgets(feedback, MAX, stdin);
                        feedback[strcspn(feedback, "\n")] = '\0';
                        fprintf(feedback_fp, "%s\n", feedback);
                        fclose(feedback_fp);
                        printf(" Thank you for your feedback!\n");
                        break;
                    case 4:
                        printf(" Logging out...\n");
                        goto end_login;
                    default:
                        printf(" Invalid choice. Try again.\n");
                        break;

                    }
                }

end_login:
end_teacher:
                ;
            }

            else

            {
                printf(" Invalid username or password.\n");
            }
            break;




        }
    }

    return 0;
}
