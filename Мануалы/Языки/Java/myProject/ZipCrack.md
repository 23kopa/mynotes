Запуск:
```java
java -cp target/ZipCrack-1.0-SNAPSHOT.jar:/home/user23/Kopilets/Java/Task2/jar/commons-compress-1.21.jar:/home/user23/Kopilets/Java/Task2/jar/zip4j-2.9.1.jar com.cracker.ZipCrack
```

root@vbox:/home/user23/Kopilets/Java/Task2# java -cp target/ZipCrack-1.0-SNAPSHOT.jar:/home/user23/Kopilets/Java/Task2/jar/commons-compress-1.21.jar:/home/user23/Kopilets/Java/Task2/jar/zip4j-2.9.1.jar com.cracker.ZipCrack
Пароль от архива: qwerty
root@vbox:/home/user23/Kopilets/Java/Task2# tree
.
├── files
│   ├── myArch.zip
│   ├── myfile.txt
│   └── pass.txt
├── jar
│   ├── commons-compress-1.21.jar
│   └── zip4j-2.9.1.jar
├── output
│   ├── hack_pass.txt
│   └── myfile.txt
├── pom.xml
├── src
│   └── main
│       └── java
│           └── com
│               └── cracker
│                   └── ZipCrack.java
└── target
    ├── classes
    │   └── com
    │       └── cracker
    │           └── ZipCrack.class
    ├── generated-sources
    │   └── annotations
    ├── maven-archiver
    │   └── pom.properties
    ├── maven-status
    │   └── maven-compiler-plugin
    │       └── compile
    │           └── default-compile
    │               ├── createdFiles.lst
    │               └── inputFiles.lst
    └── ZipCrack-1.0-SNAPSHOT.jar

20 directories, 14 files

вот мой проект, сейчас он запускается командой
java -cp target/ZipCrack-1.0-SNAPSHOT.jar:/home/user23/Kopilets/Java/Task2/jar/commons-compress-1.21.jar:/home/user23/Kopilets/Java/Task2/jar/zip4j-2.9.1.jar com.cracker.ZipCrack

как сделать, чтобы пользователь мог выбирать какой zip файл ему сканировать 

сейчас содержимое  ZipCrack.java:

```java
package com.cracker;

import java.io.*;
import java.util.*;
import net.lingala.zip4j.ZipFile;
import net.lingala.zip4j.exception.ZipException;
import net.lingala.zip4j.model.FileHeader;

public class ZipCrack {

    public static void main(String[] args) {
        String zipFilePath = "files/myArch.zip";
        String passwordFilePath = "files/pass.txt";
        String outputFilePath = "output/hack_pass.txt";

        try {
            List<String> passwords = readPasswords(passwordFilePath);

            for (String password : passwords) {
                if (tryPassword(zipFilePath, password)) {
                    writeOutput(outputFilePath, password);
                    System.out.println("Пароль от архива: " + password);
                    return;
                }
            }

            System.out.println("Пароль не найден.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static List<String> readPasswords(String filePath) throws IOException {
        List<String> passwords = new ArrayList<>();
        BufferedReader reader = new BufferedReader(new FileReader(filePath));
        String line;
        while ((line = reader.readLine()) != null) {
            passwords.add(line);
        }
        reader.close();
        return passwords;
    }

    private static boolean tryPassword(String zipFilePath, String password) {
        try {
            ZipFile zipFile = new ZipFile(zipFilePath);
            zipFile.setPassword(password.toCharArray());

            List<FileHeader> fileHeaders = zipFile.getFileHeaders();
            for (FileHeader fileHeader : fileHeaders) {
                zipFile.extractFile(fileHeader, "output/");
            }

            return true;
        } catch (ZipException e) {
            // Handle exception
        }
        return false;
    }

    private static void writeOutput(String filePath, String password) throws IOException {
        BufferedWriter writer = new BufferedWriter(new FileWriter(filePath));
        writer.write("Пароль от архива: " + password);
        writer.newLine();
        writer.close();
    }
}
```
***

```java
package com.cracker;

import java.io.*;
import java.util.*;
import java.util.Scanner;

import net.lingala.zip4j.ZipFile;
import net.lingala.zip4j.exception.ZipException;
import net.lingala.zip4j.model.FileHeader;

public class ZipCrack {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Введите путь к ZIP-файлу: ");
        String zipFilePath = scanner.nextLine(); // Получаем путь к ZIP-файлу от пользователя
        scanner.close();
        
        String passwordFilePath = "files/pass.txt";
        String outputFilePath = "output/hack_pass.txt";

        try {
            List<String> passwords = readPasswords(passwordFilePath);

            for (String password : passwords) {
                if (tryPassword(zipFilePath, password)) {
                    writeOutput(outputFilePath, password);
                    System.out.println("Пароль от архива: " + password);
                    return;
                }
            }

            System.out.println("Пароль не найден.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static List<String> readPasswords(String filePath) throws IOException {
        List<String> passwords = new ArrayList<>();
        BufferedReader reader = new BufferedReader(new FileReader(filePath));
        String line;
        while ((line = reader.readLine()) != null) {
            passwords.add(line);
        }
        reader.close();
        return passwords;
    }

    private static boolean tryPassword(String zipFilePath, String password) {
        try {
            ZipFile zipFile = new ZipFile(zipFilePath);
            zipFile.setPassword(password.toCharArray());

            List<FileHeader> fileHeaders = zipFile.getFileHeaders();
            for (FileHeader fileHeader : fileHeaders) {
                zipFile.extractFile(fileHeader, "output/");
            }

            return true;
        } catch (ZipException e) {
            // Handle exception
        }
        return false;
    }

    private static void writeOutput(String filePath, String password) throws IOException {
        BufferedWriter writer = new BufferedWriter(new FileWriter(filePath));
        writer.write("Пароль от архива: " + password);
        writer.newLine();
        writer.close();
    }
}

```

```
mvn package
```