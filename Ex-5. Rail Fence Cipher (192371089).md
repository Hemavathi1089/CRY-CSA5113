#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <math.h>

// Function to encrypt using Rail Fence (row & column transformation)
void encrypt(char *plaintext, int rows, char *ciphertext) {
    int len = strlen(plaintext);
    int cols = ceil((float)len / rows);
    char matrix[rows][cols];

    // Initialize matrix with filler
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            matrix[i][j] = 'X';

    // Fill matrix row-wise
    int k = 0;
    for (int i = 0; i < rows && k < len; i++) {
        for (int j = 0; j < cols && k < len; j++) {
            if (isalpha(plaintext[k])) {
                matrix[i][j] = toupper(plaintext[k++]);
            } else {
                k++; // Skip non-alphabetic
                j--; // Retry this column
            }
        }
    }

    // Read matrix column-wise to get ciphertext
    k = 0;
    for (int j = 0; j < cols; j++)
        for (int i = 0; i < rows; i++)
            ciphertext[k++] = matrix[i][j];

    ciphertext[k] = '\0';
}

// Function to decrypt (reverse transformation)
void decrypt(char *ciphertext, int rows, char *plaintext) {
    int len = strlen(ciphertext);
    int cols = ceil((float)len / rows);
    char matrix[rows][cols];

    // Fill matrix column-wise
    int k = 0;
    for (int j = 0; j < cols && k < len; j++) {
        for (int i = 0; i < rows && k < len; i++) {
            matrix[i][j] = ciphertext[k++];
        }
    }

    // Read matrix row-wise to get plaintext
    k = 0;
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            plaintext[k++] = matrix[i][j];

    plaintext[k] = '\0';
}

// Main Function
int main() {
    char plaintext[100], ciphertext[100], decrypted[100];
    int rows;

    printf("Enter the plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    printf("Enter number of rows (key): ");
    scanf("%d", &rows);

    encrypt(plaintext, rows, ciphertext);
    printf("Encrypted Text: %s\n", ciphertext);

    decrypt(ciphertext, rows, decrypted);
    printf("Decrypted Text: %s\n", decrypted);

    return 0;
}
