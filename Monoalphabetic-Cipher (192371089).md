#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <stdlib.h>

#define MAX_LEN 1000
#define ALPHABET_SIZE 26
char english_freq_order[ALPHABET_SIZE] = {
    'e', 't', 'a', 'o', 'i', 'n', 's', 'h', 'r', 'd',
    'l', 'c', 'u', 'm', 'w', 'f', 'g', 'y', 'p', 'b',
    'v', 'k', 'j', 'x', 'q', 'z'
};
typedef struct {
    char letter;
    int count;
} Freq;
int compare_freq(const void *a, const void *b) {
    return ((Freq *)b)->count - ((Freq *)a)->count;
}
void decrypt_mono(char *ciphertext, char map[26], char *plaintext) {
    for (int i = 0; ciphertext[i]; i++) {
        if (isalpha(ciphertext[i])) {
            char c = tolower(ciphertext[i]);
            plaintext[i] = map[c - 'a'];
        } else {
            plaintext[i] = ciphertext[i];
        }
    }
    plaintext[strlen(ciphertext)] = '\0';
}
int score_text(char *text) {
    int score = 0;
    char common[] = "theandofin";
    for (int i = 0; text[i]; i++) {
        if (strchr(common, tolower(text[i]))) score++;
    }
    return score;
}
int main () {
    char ciphertext [MAX_LEN];
    Freq freq[ALPHABET_SIZE] = {0};
    char map [ALPHABET_SIZE];
    int topN;
    printf("Enter ciphertext: ");
    fgets(ciphertext, MAX_LEN, stdin);
    ciphertext [strcspn(ciphertext, "\n")] = '\0';
    printf("How many top plaintexts to show? ");
    scanf("%d", &topN);
    for (int i = 0; i < ALPHABET_SIZE; i++) {
        freq[i]. letter = 'a' + i;
        freq[i]. count = 0;
    }
    for (int i = 0; ciphertext[i]; i++) {
        char c = tolower(ciphertext[i]);
        if (isalpha(c)) {
            freq[c - 'a']. count++;
        }
    }
    qsort(freq, ALPHABET_SIZE, sizeof(Freq), compare_freq);
    for (int i = 0; i < ALPHABET_SIZE; i++) {
        map[freq[i].letter - 'a'] = english_freq_order[i];
    }
    char plaintext[MAX_LEN];
    decrypt_mono(ciphertext, map, plaintext);
    printf("\nLikely Plaintext:\n%s\n", plaintext);
    printf("\nNote: This output is based on unigram frequency and may not be 100%% accurate.\n");
    return 0;
}
