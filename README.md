

<details>
<summary>Code example🖥️ </summary>

    
    #include <iostream>
    #include <WinSock2.h>
    #include <Ws2tcpip.h>
    #pragma comment(lib, "ws2_32.lib")
    
    int main() {
    WSADATA wsaData;
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) {
        std::cerr << "WSAStartup failed" << std::endl;
        return 1;
    }

    int port = 15010;
    PCWSTR serverIp = L"3.71.225.231";

    // Create a TCP socket
    SOCKET clientSocket = socket(AF_INET, SOCK_STREAM, 0);
    if (clientSocket == INVALID_SOCKET) {
        std::cerr << "Error creating socket: " << WSAGetLastError() << std::endl;
        WSACleanup();
        return 1;
    }

    sockaddr_in serverAddr;
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_port = htons(port);
    InetPton(AF_INET, serverIp, &serverAddr.sin_addr);

    if (connect(clientSocket, reinterpret_cast<sockaddr*>(&serverAddr), sizeof(serverAddr)) == SOCKET_ERROR) {
        std::cerr << "Connect failed with error: " << WSAGetLastError() << std::endl;
        closesocket(clientSocket);
        WSACleanup();
        return 1;
    }

    // Шістнадцяткове число
    uint32_t hexNumber = 0x1234; // Це число в шістнадцятковій формі

    // Перетворення в мережевий порядок байтів
    uint32_t hexNumberNetwork = htonl(hexNumber);

    // Відправка шістнадцяткового числа (передача 4 байт)
    send(clientSocket, reinterpret_cast<const char*>(&hexNumberNetwork), sizeof(hexNumberNetwork), 0);

    // Отримання відповіді від сервера
    char buffer[1024];
    memset(buffer, 0, sizeof(buffer));
    int byteReceived = recv(clientSocket, buffer, sizeof(buffer), 0);
    if (byteReceived > 0) {
        std::cout << "Received from server: " << buffer << std::endl;
    }

    // Clean up
    closesocket(clientSocket);
    WSACleanup();
    return 0;
}


```


```
</details>
