#include <windows.h>
#include <iostream>
#include <fstream>

const wchar_t* MUTEX_NAME = L"SharedMutex";
const wchar_t* SHARED_FILE = L"shared_data.txt";

void readFromFile() {
    std::wifstream inFile(SHARED_FILE);
    if (inFile.is_open()) {
        std::wstring data;
        std::getline(inFile, data);
        std::wcout << L"Data read from file: " << data << std::endl;
        inFile.close();
    }
}

int main() {
    HANDLE hMutex = OpenMutexW(MUTEX_ALL_ACCESS, FALSE, MUTEX_NAME);
    if (hMutex == NULL) {
        return 1;
    }

  readFromFile();

ReleaseMutex(hMutex);

CloseHandle(hMutex);
    return 0;
}
