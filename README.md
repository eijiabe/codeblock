# codeblock

#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat file_info;

    // Abre o diretório C:\ para leitura
    dir = opendir("C:\\");
    if (!dir) {
        perror("Erro ao abrir diretório");
        exit(1);
    }

    // Percorre cada entrada do diretório
    while ((entry = readdir(dir)) != NULL) {
        // Ignora as entradas "." e ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {
            continue;
        }

        // Cria o caminho completo para o arquivo/pasta
        char path[256];
        sprintf(path, "C:\\%s", entry->d_name);

        // Obtém informações sobre o arquivo/pasta
        if (stat(path, &file_info) < 0) {
            perror("Erro ao obter informações do arquivo");
            continue;
        }

        // Verifica se é um arquivo
        if (S_ISREG(file_info.st_mode)) {
            // Cria o novo nome com a extensão .bin
            char new_path[256];
            sprintf(new_path, "C:\\%s.bin", entry->d_name);

            // Renomeia o arquivo
            if (rename(path, new_path) < 0) {
                perror("Erro ao renomear arquivo");
            } else {
                printf("Arquivo %s renomeado para %s\n", path, new_path);
            }
        }
    }

    // Fecha o diretório
    closedir(dir);

    return 0;
}
