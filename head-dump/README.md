# README: Solving the picoCTF Challenge - head-dump

## Challenge Overview

**Challenge:** head-dump  
**Category:** Web Exploitation  
**Author:** St01  
**Points:** 100 (Assumed)

In this challenge, you need to explore an article about API documentation to find an endpoint that generates files holding the server’s memory. Your task is to find the flag hidden within the memory dump.

---

## Steps to Solve the Challenge

### 1. Access the Target URL

The first step is to access the provided target URL. Upon visiting the page, you’ll encounter a basic article, which contains several hyperlinks and a hashtag called **#API Documentation**.

### 2. Navigate to the API Documentation

Click on the **#API Documentation** link. This will take you to the section containing details about the API and its endpoints. Here, we are interested in finding an endpoint that generates files containing memory data.

### 3. Look for the "Diagnosing" Section

In the API documentation, find the section titled **Diagnosing**. This section is essential because it allows you to interact with the server and potentially retrieve memory-related data, which could contain the flag.

### 4. Use the API to Retrieve the Memory File

- Within the **Diagnosing** section, click **Try it out** to interact with the endpoint.
- Execute the endpoint by clicking the **Execute** button. This will generate a file related to the server’s memory, which can be downloaded.
- Download the file and save it to your local machine for analysis.

### 5. Open the File

After downloading the file, open it using **TextEdit** on macOS.

### 6. Search for the Flag

Once the file is opened, press **Command + F** to bring up the search function in TextEdit. Search for the word **"pico"**. This will bring you to the part of the file where the flag is hidden.

### 7. Extract the Flag

The flag should appear in the search results. The flag format is:

```
picoCTF{Pat!3nt_15_Th3_K3y_bed6b6b8}
```

## Flag Found

```
picoCTF{Pat!3nt_15_Th3_K3y_bed6b6b8}
```

## Conclusion

This challenge was a simple web exploitation task where the goal was to use the API to generate a file containing the server’s memory, then search the file for the flag. The key takeaway is to use API documentation effectively to interact with endpoints that can provide useful information for capturing flags.