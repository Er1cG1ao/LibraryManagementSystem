# LibraryManagementSystem
my code:
```java
/**
 * LibraryManagementSystem
 * @description
 * This is a console-based Java written Library Management System designed to help libraries manage their book inventory efficiently. 
 * It allows users to record, sort, borrow, search, remove, and analyze books. 
 * The program uses 2D arrays to store book details, including the book’s name, author, year of publication, stock, and borrowed count.
 * @author Er1cG1ao
 * @version 1.0.0
 */
import java.util.Scanner; //importing scanner class
public class LibraryManagementSystem
{
    private static String[][] nameAndAuthor;
    private static int[][] publishStockBorrow;
    private static int tableWidth=40;
    public static void main(String args[])
    {
        System.out.println("Program starting up...");
        System.out.println("\n----------------------------");
        System.out.println("LibraryManagementSystem");
        System.out.println("Version: 1.0.0");
        System.out.println("Author: Er1cG1ao");
        System.out.println("Program description: This is a console-based Java written Library Management System designed to help libraries manage their book inventory efficiently. ");
        System.out.println("It allows users to record, sort, borrow, search, remove, and analyze books. ");
        System.out.println("The program uses 2D arrays to store book details, including the book’s name, author, year of publication, stock, and borrowed count.");
        System.out.println("----------------------------");
        System.out.println("\nYou will now be asked to initialize the bookshelf");
        initializeBookArray();
    }

    /* initialize array */
    public static void initializeBookArray()
    {
        Scanner input=new Scanner(System.in);
        System.out.println("\n"+"-".repeat(tableWidth));//will be modified once table is complete
        System.out.println("Initializing bookshelf");
        System.out.print("\nHow many different books do you have? ");
        int numOfBooks = 0;
        while(!input.hasNextInt()) { // type safe
            input.nextLine();
            System.out.println("Invalid input, please input an integer");
        }
        numOfBooks = input.nextInt();
        numOfBooks = Math.abs(numOfBooks);//incase the user input a negative number. Error handling
        input.nextLine(); 

        nameAndAuthor = new String[numOfBooks][2]; // Each book is a row: [book][0]=Name, [book][1]=Author
        publishStockBorrow = new int[numOfBooks][3];  // Each book is a row: [book][0]=Year, [book][1]=Stock, [book][2]=Borrowed
        for(int book = 0; book < numOfBooks; book++) {
            System.out.print("Name of book "+(book+1)+": ");
            nameAndAuthor[book][0] = input.nextLine(); // Name of the book
            System.out.print("Name of the author: ");
            nameAndAuthor[book][1] = input.nextLine(); // Author of the book
            System.out.print("Year of publish: ");
            while(!input.hasNextInt()) {
                input.nextLine();
                System.out.println("Invalid input, please enter an integer.");
            }
            publishStockBorrow[book][0] = input.nextInt(); // Year of publication
            input.nextLine();
            System.out.print("Number of this book on the shelf: ");
            while(!input.hasNextInt()) {
                input.nextLine();
                System.out.println("Invalid input, please enter an integer.");
            }
            publishStockBorrow[book][1] = input.nextInt(); // Stock quantity
            input.nextLine(); 
            publishStockBorrow[book][2] = 0; // Initialize borrowed count to 0
            System.out.print("\n");
        }
        System.out.println("Bookshelf initialized:");
        showBookshelf(nameAndAuthor, publishStockBorrow);
        showMenu();
    }

    /* add books */
    public static void addBooks() {
        Scanner input = new Scanner(System.in);
        boolean addingBooks = true;

        while (addingBooks) {
            System.out.print("Enter the name of the book: ");
            String bookName = input.nextLine().trim();

            System.out.print("Enter the author's name: ");
            String authorName = input.nextLine().trim();

            // Check if book already exists
            boolean bookExists = false;
            for (int i = 0; i < nameAndAuthor.length; i++) {
                if (nameAndAuthor[i][0].equalsIgnoreCase(bookName) && nameAndAuthor[i][1].equalsIgnoreCase(authorName)) {//name same author same
                    bookExists = true;
                    System.out.print("This book is already on the shelf. How many more copies are you adding? ");
                    while (!input.hasNextInt()) {//type safe
                        input.nextLine();
                        System.out.println("Invalid input, please enter an integer.");
                    }
                    int additionalStock = input.nextInt();
                    input.nextLine(); // eat user's extra enter
                    publishStockBorrow[i][1] += additionalStock; // Update stock column [i]is the book, [1]is the column for stock
                    System.out.println("Stock updated. Total copies now: " + publishStockBorrow[i][1]);
                    break;
                }
            }

            if (!bookExists) {//new book then
                System.out.print("Enter the year of publication: ");
                while (!input.hasNextInt()) {
                    input.nextLine();
                    System.out.println("Invalid input, please enter an integer.");
                }
                int year = input.nextInt();
                input.nextLine();

                System.out.print("Enter the stock quantity: ");
                while (!input.hasNextInt()) {
                    input.nextLine();
                    System.out.println("Invalid input, please enter an integer.");
                }
                int stock = input.nextInt();
                input.nextLine();

                int currentBooks = nameAndAuthor.length;//number of unique books we have right now
                int newTotalBooks = currentBooks + 1;//adding that new book

                // Create new arrays with increased size
                String[][] newNameAndAuthor = new String[newTotalBooks][2];
                int[][] newPublishStockBorrow = new int[newTotalBooks][3];

                // Copy old data into new arrays
                for (int i = 0; i < currentBooks; i++) {//since we only have 5 items to copy, i am not using nested for loops
                    newNameAndAuthor[i][0] = nameAndAuthor[i][0];
                    newNameAndAuthor[i][1] = nameAndAuthor[i][1];

                    newPublishStockBorrow[i][0] = publishStockBorrow[i][0];
                    newPublishStockBorrow[i][1] = publishStockBorrow[i][1];
                    newPublishStockBorrow[i][2] = publishStockBorrow[i][2];
                }

                // assigning values of the new book
                newNameAndAuthor[currentBooks][0] = bookName;
                newNameAndAuthor[currentBooks][1] = authorName;
                newPublishStockBorrow[currentBooks][0] = year;
                newPublishStockBorrow[currentBooks][1] = stock;
                newPublishStockBorrow[currentBooks][2] = 0; //new book has borrow 0

                //replace adress
                nameAndAuthor = newNameAndAuthor;
                publishStockBorrow = newPublishStockBorrow;

                System.out.println("Book added successfully!");
            }

            System.out.print("\nDo you want to add another book? (yes/no): ");
            String response = input.nextLine().trim();
            if (!response.equals("yes")) {//if not yes
                addingBooks = false;
            }
        }

        System.out.println("\nBookshelf updated:");
        showBookshelf(nameAndAuthor, publishStockBorrow);
    }

    /* Removing books  */
    public static void removeBooks() {
        Scanner input = new Scanner(System.in);
        boolean removingBooks = true;

        while (removingBooks) {
            System.out.print("Enter the name of the book you want to remove: ");
            String bookName = input.nextLine().trim();//we don't want extra not needed spaces
            System.out.print("Enter the name of the author: ");
            String author = input.nextLine().trim();//we don't want extra not needed spaces
            boolean bookFound = false;
            for (int i = 0; i < nameAndAuthor.length; i++) {//go through the whole bookshelf
                if (nameAndAuthor[i][0].equalsIgnoreCase(bookName)&&nameAndAuthor[i][1].equalsIgnoreCase(author)) {//if found the same name and author
                    bookFound = true;
                    System.out.println("\nThere are currently "+publishStockBorrow[i][1]+" copies of this book.");//tell the user how much left
                    System.out.print("Enter the number of copies to remove: ");
                    while (!input.hasNextInt()) {//type safe
                        input.nextLine();
                        System.out.println("Invalid input, please enter an integer.");
                    }
                    int removeAmount = input.nextInt();
                    input.nextLine();

                    if (removeAmount < publishStockBorrow[i][1]) {//so if there are more stock than what has to be removed
                        // Reduce stock
                        publishStockBorrow[i][1] -= removeAmount;
                        System.out.println("\nStock updated. Remaining copies: " + publishStockBorrow[i][1]);//tell the user the current stock
                    } else {//means the user is removing more than what is currently in stock
                        // Check if any books are currently borrowed, to decide if we want to clear all the info of this book
                        if (publishStockBorrow[i][2] > 0) {//if there is borrowed coppy
                            System.out.println("\nCannot remove completely. Some copies are currently borrowed.");
                            System.out.println("Stock remains: " + publishStockBorrow[i][1]);//again to update the user about the stock
                        } else {//if there is no more copies being borrowd
                            // we want to remove the whole row about this book.
                            int totalBooks = nameAndAuthor.length;
                            String[][] newNameAndAuthor = new String[totalBooks - 1][2];//temp new arrays
                            int[][] newPublishStockBorrow = new int[totalBooks - 1][3];

                            for (int j = 0, k = 0; j < totalBooks; j++) {//normally increasing j which is for the original arrays
                                if (j != i) {//when the current row is not the row of the book being removed, we do want to copy. other wise we don't copy but let only j to increase
                                    newNameAndAuthor[k][0] = nameAndAuthor[j][0];
                                    newNameAndAuthor[k][1] = nameAndAuthor[j][1];
                                    newPublishStockBorrow[k][0] = publishStockBorrow[j][0];
                                    newPublishStockBorrow[k][1] = publishStockBorrow[j][1];
                                    newPublishStockBorrow[k][2] = publishStockBorrow[j][2];
                                    k++;
                                }
                            }
                            //replacing adress
                            nameAndAuthor = newNameAndAuthor;
                            publishStockBorrow = newPublishStockBorrow;

                            System.out.println("\nInformation of this book has been completely removed from the library.");
                        }
                    }
                    break;
                }
            }
            if (!bookFound) {//if book is not found
                System.out.println("Book not found on the shelf.");
            }
            // Ask user if they want to remove another book
            System.out.print("Do you want to remove another book? (yes/no): ");
            String response = input.nextLine().trim().toLowerCase();
            if (!response.equals("yes")) {//if not yes then stop removing. This way we save two more if statements("no" and anything else)
                removingBooks = false;
            }
        }

        // Display updated bookshelf
        System.out.println("\nBookshelf updated:");
        showBookshelf(nameAndAuthor, publishStockBorrow);
    }

    /* Sorting Controller */
    public static void sortBooks() {
        Scanner input = new Scanner(System.in);
        boolean sorted = false;
        while (!sorted) {
            System.out.print("How would you like to sort? (name/year/stock) ");
            String usrInput = input.nextLine().trim();
            switch (usrInput) {//switch case to see how the user want to sort the shelf
                case "name":
                    sortStringBubble(nameAndAuthor, publishStockBorrow, 0);//0 because we are sorting base on column 0 of String array
                    sorted = true;
                    break;
                case "year":
                    System.out.print("In ascending order (yes/no)? ");
                    String usrInput2 = input.nextLine().trim();
                    if (usrInput2.equals("yes")) {
                        sortAscIntBubble(nameAndAuthor, publishStockBorrow, 0); // Year is column 0
                    } else if (usrInput2.equals("no")) {
                        sortDesIntBubble(nameAndAuthor, publishStockBorrow, 0);
                    }
                    sorted = true;
                    break;
                case "stock":
                    System.out.print("In ascending order (yes/no)? ");
                    String usrInput3 = input.nextLine().trim();
                    if (usrInput3.equals("yes")) {
                        sortAscIntBubble(nameAndAuthor, publishStockBorrow, 1); // Stock is column 1
                    } else if (usrInput3.equals("no")) {
                        sortDesIntBubble(nameAndAuthor, publishStockBorrow, 1);
                    }
                    sorted = true;
                    break;
                default:
                    System.out.println("Received input doesn't match any options, try again.");
            }
        }
        showBookshelf(nameAndAuthor, publishStockBorrow);//show the updated bookshelf
    }

    /* Bubble Sort based on String (for sorting by Name) */
    public static void sortStringBubble(String[][] nameAndAuthor, int[][] publishStockBorrow, int indexOfSortBase) {
        int n = nameAndAuthor.length; // Number of books (rows)
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - 1 - i; j++) {
                if (nameAndAuthor[j][indexOfSortBase].compareToIgnoreCase(nameAndAuthor[j + 1][indexOfSortBase]) > 0) {//current one is greater than next one
                    // Swap entire rows in nameAndAuthor
                    String[] tempRowStr = nameAndAuthor[j];
                    nameAndAuthor[j] = nameAndAuthor[j + 1];
                    nameAndAuthor[j + 1] = tempRowStr;

                    // Swap entire rows in publishStockBorrow
                    int[] tempRowInt = publishStockBorrow[j];
                    publishStockBorrow[j] = publishStockBorrow[j + 1];
                    publishStockBorrow[j + 1] = tempRowInt;
                }
            }
        }
    }

    /* Ascending Bubble Sort for Integers (for sorting by Year/Stock) */
    public static void sortAscIntBubble(String[][] nameAndAuthor, int[][] publishStockBorrow, int indexOfSortBase) {
        int n = publishStockBorrow.length; // Number of books (rows)
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - 1 - i; j++) {
                if (publishStockBorrow[j][indexOfSortBase] > publishStockBorrow[j + 1][indexOfSortBase]) {
                    //swaping string parts
                    String[] tempRowStr = new String[nameAndAuthor[j].length];
                    for (int k = 0; k < nameAndAuthor[j].length; k++) {
                        tempRowStr[k] = nameAndAuthor[j][k];
                    }
                    for (int k = 0; k < nameAndAuthor[j].length; k++) {
                        nameAndAuthor[j][k] = nameAndAuthor[j + 1][k];
                        nameAndAuthor[j + 1][k] = tempRowStr[k];
                    }
                    //swaping int parts
                    int[] tempRowInt = new int[publishStockBorrow[j].length];
                    for (int k = 0; k < publishStockBorrow[j].length; k++) {
                        tempRowInt[k] = publishStockBorrow[j][k];
                    }
                    for (int k = 0; k < publishStockBorrow[j].length; k++) {
                        publishStockBorrow[j][k] = publishStockBorrow[j + 1][k];
                        publishStockBorrow[j + 1][k] = tempRowInt[k];
                    }
                }
            }
        }
    }

    /* Descending Bubble Sort for Integers (for sorting by Year/Stock) */
    public static void sortDesIntBubble(String[][] nameAndAuthor, int[][] publishStockBorrow, int indexOfSortBase) {
        int n = publishStockBorrow.length; // Number of books (rows)
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - 1 - i; j++) {
                if (publishStockBorrow[j][indexOfSortBase] < publishStockBorrow[j + 1][indexOfSortBase]) {
                    //swaping string parts
                    String[] tempRowStr = new String[nameAndAuthor[j].length];
                    for (int k = 0; k < nameAndAuthor[j].length; k++) {
                        tempRowStr[k] = nameAndAuthor[j][k];
                    }
                    for (int k = 0; k < nameAndAuthor[j].length; k++) {
                        nameAndAuthor[j][k] = nameAndAuthor[j + 1][k];
                        nameAndAuthor[j + 1][k] = tempRowStr[k];
                    }
                    //swaping int parts
                    int[] tempRowInt = new int[publishStockBorrow[j].length];
                    for (int k = 0; k < publishStockBorrow[j].length; k++) {
                        tempRowInt[k] = publishStockBorrow[j][k];
                    }
                    for (int k = 0; k < publishStockBorrow[j].length; k++) {
                        publishStockBorrow[j][k] = publishStockBorrow[j + 1][k];
                        publishStockBorrow[j + 1][k] = tempRowInt[k];
                    }
                }
            }
        }
    }
    /* show book shelf */
    public static void showBookshelf(String[][] nameAndAuthor, int[][] publishStockBorrow) {
        int[] columnLengths = new int[5];//array for storing lengths of each columns
        for (int col = 0; col < 2; col++) { // Calculate max length for Name and Author columns
            for (int book = 0; book < nameAndAuthor.length; book++) {//from name to author, this is specifically for String array
                if (nameAndAuthor[book][col].length() > columnLengths[col]) {//comparing to find the max length
                    columnLengths[col] = nameAndAuthor[book][col].length();
                }
            }
        }
        // Calculate max length for Year, Stock, and Borrowed columns
        for (int book = 0; book < publishStockBorrow.length; book++) {//from year to stock to borrow
            for (int col = 0; col < 3; col++) {//the three columns
                int num = publishStockBorrow[book][col];//the value of current book, can be year/stock/borrow
                int numLength = 0;//length of the num

                if (num < 0) {//if value is negative
                    numLength++; // Add extra space for the negative sign
                    num = -num; // set the negative value to positive
                }

                while (num > 0) {
                    numLength++; // add one digit to the length of this column
                    num /= 10; // remove a digit
                }

                if (numLength > columnLengths[col + 2]) {
                    columnLengths[col + 2] = numLength;
                }
            }
        }

        //if max length calculated base on values are shorter than headers, use header's length. vise versa
        columnLengths[0] = Math.max(columnLengths[0], "Name".length());
        columnLengths[1] = Math.max(columnLengths[1], "Author".length());
        columnLengths[2] = Math.max(columnLengths[2], "Year".length());
        columnLengths[3] = Math.max(columnLengths[3], "Stock".length());
        columnLengths[4] = Math.max(columnLengths[4], "Borrowed".length());

        int nameColLength = columnLengths[0];
        int authorColLength = columnLengths[1];
        int yearColLength = columnLengths[2];
        int stockColLength = columnLengths[3];
        int borrowColLength = columnLengths[4];

        // Calculate the total table width
        tableWidth = nameColLength + authorColLength + yearColLength + stockColLength + borrowColLength + 16;
        // Print top border
        System.out.println("-".repeat(tableWidth));
        // Print table headers
        System.out.printf("| %-"+nameColLength+"s | %-"+authorColLength+"s | %-"+yearColLength+"s | %-"+stockColLength+"s | %-"+borrowColLength+"s |\n",
            "Name", "Author", "Year", "Stock", "Borrowed");
        System.out.println("|" + "-".repeat(nameColLength+2) + "|" + "-".repeat(authorColLength+2) + "|" +
            "-".repeat(yearColLength+2) + "|" + "-".repeat(stockColLength+2) + "|" + "-".repeat(borrowColLength+2) + "|");

        // print out the book data
        for (int i = 0; i < nameAndAuthor.length; i++) {
            System.out.printf("| %-"+nameColLength+"s | %-"+authorColLength+"s | %-"+yearColLength+"d | %-"+stockColLength+"d | %-"+borrowColLength+"d |\n",
                nameAndAuthor[i][0], nameAndAuthor[i][1], publishStockBorrow[i][0], publishStockBorrow[i][1], publishStockBorrow[i][2]);
        }
    }
    /* Search books by name */
    public static void searchBooks() {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter the name of the book you want to search for: ");
        String bookName = input.nextLine().trim();//remove extra space
        // Count matching books
        int matchCount = 0;
        for (int bookIndex = 0; bookIndex < nameAndAuthor.length; bookIndex++) {
            if (nameAndAuthor[bookIndex][0].equalsIgnoreCase(bookName)) {
                matchCount++;
            }
        }
        // If no matches, inform user and return
        if (matchCount == 0) {
            System.out.println("No book found with the name: " + bookName);
            return;//bring user back to menu
        }
        // Create new arrays to store matching books
        String[][] matchedNameAndAuthor = new String[matchCount][2];
        int[][] matchedPublishStockBorrow = new int[matchCount][3];
        // put info of found books into the new arrays
        int matchedBookCount = 0;
        for (int bookIndex = 0; bookIndex < nameAndAuthor.length; bookIndex++) {
            if (nameAndAuthor[bookIndex][0].equalsIgnoreCase(bookName)) {//if found, we put that book into the new arrays
                matchedNameAndAuthor[matchedBookCount][0] = nameAndAuthor[bookIndex][0]; // Book Name
                matchedNameAndAuthor[matchedBookCount][1] = nameAndAuthor[bookIndex][1]; // Author
                matchedPublishStockBorrow[matchedBookCount][0] = publishStockBorrow[bookIndex][0]; // Year
                matchedPublishStockBorrow[matchedBookCount][1] = publishStockBorrow[bookIndex][1]; // Stock
                matchedPublishStockBorrow[matchedBookCount][2] = publishStockBorrow[bookIndex][2]; // Borrowed
                matchedBookCount++;//to the next row
            }
        }
        // Display the search results using showBookshelf
        System.out.println("\nSearch results for '" + bookName + "':");
        showBookshelf(matchedNameAndAuthor, matchedPublishStockBorrow);//using new arrays instead of the full array to show only founded books
    }

    /* Borrow book function */
    public static void borrowBook() {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter the name of the book you want to borrow: ");
        String bookName = input.nextLine().trim();

        boolean bookFound = false;
        boolean authorMismatch = false;

        for (int i = 0; i < nameAndAuthor.length; i++) {//check every row
            if (nameAndAuthor[i][0].equalsIgnoreCase(bookName)) {//if found the name on the shelf
                bookFound = true;
                System.out.print("Enter the author's name: ");//to check if author also matchs
                String authorName = input.nextLine().trim();

                if (nameAndAuthor[i][1].equalsIgnoreCase(authorName)) {// Name and author both match
                    if (publishStockBorrow[i][1] > 0) {// means there is available copy
                        publishStockBorrow[i][1]--; // Decrease stock
                        publishStockBorrow[i][2]++; // Increase borrow
                        System.out.println("\nYou have successfully borrowed the book.");
                        showBookshelf(nameAndAuthor, publishStockBorrow);
                    } else if (publishStockBorrow[i][2] > 0) {// no stock left but borrowed copies exist
                        System.out.println("\nAll copies of this book have already been borrowed.");
                        showBookshelf(nameAndAuthor, publishStockBorrow);
                    } else {// Stock is 0 and no borrowed copies exist
                        System.out.println("\nThis book is currently unavailable.");//this normally won't happen because remove book function won't let this
                    }
                    return;//prevent more unneccessayr checks
                } else {//if name same but author diffenrt
                    authorMismatch = true;//special case of name exist but author don't
                }
            }
        }

        if (!bookFound) {//didn't find the book
            System.out.println("No book found with the name: " + bookName);
        } else if (authorMismatch) {//found it but author don't match
            System.out.println("A book with this name exists, but the author does not match.");
        }
    }
    /* show stats */
    public static void showStatistics()//totals, min list and max list, avg publish year
    {
        int totalBooksOnShelf = 0;
        int totalBooksOwned = 0;
        int minStock = publishStockBorrow[0][1];
        int maxStock = publishStockBorrow[0][1];
        int[] years = new int[nameAndAuthor.length];
        String[] maxStockBooks = new String[nameAndAuthor.length];//if there are mulitple books that is max stock
        String[] minStockBooks = new String[nameAndAuthor.length];//if there are mulitple books that is min stock
        int yearCount = 0;
        int maxCount = 0;//used as index for new arrays
        int minCount = 0;//same as maxCount

        for (int i = 0; i < nameAndAuthor.length; i++) {//for every book on the shelf
            int stock = publishStockBorrow[i][1];
            int borrowed = publishStockBorrow[i][2];
            totalBooksOnShelf += stock;
            totalBooksOwned += stock + borrowed;//this one counts the borrowd book as well
            // Find books with the maximum stock
            if (stock > maxStock) { // stock is the stock of the current book being assessed
                maxStock = stock; // replace with new maximum stock
                maxCount = 0; // Reset counter since a new max is found
                maxStockBooks = new String[nameAndAuthor.length]; // Reset array to remove old values
                // Store the first book with the new max stock
                maxStockBooks[maxCount] = nameAndAuthor[i][0] + " by " + nameAndAuthor[i][1];
                maxCount++;
            } else if (stock == maxStock) {
                // Store additional books with the same max stock
                maxStockBooks[maxCount] = nameAndAuthor[i][0] + " by " + nameAndAuthor[i][1];
                maxCount++;
            }
            // Find books with the minimum stock
            if (stock < minStock) {
                minStock = stock;
                minCount = 0; // Reset counter since a new min is found
                minStockBooks = new String[nameAndAuthor.length]; // Reset array to remove old values

                // Store the first book with this new min stock
                minStockBooks[minCount] = nameAndAuthor[i][0] + " by " + nameAndAuthor[i][1];
                minCount++; //move to next index
            } else if (stock == minStock) {
                // Store additional books with the same min stock
                minStockBooks[minCount] = nameAndAuthor[i][0] + " by " + nameAndAuthor[i][1];
                minCount++;
            }
            //year
            int bookYear = publishStockBorrow[i][0];//Get the year of the current book
            years[yearCount] = bookYear;//Store the year in the years array
            yearCount++;//to know how much different year values in total
        }
        // Calculate the average publication year
        int totalYear = 0;
        for (int j = 0; j < yearCount; j++) {
            totalYear += years[j];//add up every elements in years[]
        }
        double avgYear;
        if (yearCount > 0) {
            avgYear = (double) totalYear / yearCount; //explicitly promote the operation to double
        } else {
            avgYear = 0;
        }
        // Display Statistics
        System.out.println("\n"+"-".repeat(tableWidth));//boarder
        System.out.println("Library Statistics");//title
        System.out.println("Total books on shelf (excluding borrowed): " + totalBooksOnShelf);
        System.out.println("Total books owned (including borrowed): " + totalBooksOwned);
        String maxStockBooksStr = "";
        if (maxCount == 1) {
            maxStockBooksStr = maxStockBooks[0];//if there is only one max, just assign right away
        } else {
            for (int i = 0; i < maxCount; i++) {//if multiple max exist
                if (i > 0) maxStockBooksStr += ", ";
                maxStockBooksStr += maxStockBooks[i];
            }
        }
        String minStockBooksStr = "";
        if (minCount == 1) {
            minStockBooksStr = minStockBooks[0];//same as max
        } else {
            for (int i = 0; i < minCount; i++) {//if multiple min exist
                if (i > 0) minStockBooksStr += ", ";
                minStockBooksStr += minStockBooks[i];
            }
        }
        System.out.println("Book(s) with the maximum stock (" + maxStock + " copies): " + maxStockBooksStr);
        System.out.println("Book(s) with the minimum stock (" + minStock + " copies): " + minStockBooksStr);
        System.out.printf("Average publication year: %.2f\n", avgYear);//not printing out all the decimal places
    }
    /* menu */
    public static void showMenu()
    {
        Scanner input=new Scanner(System.in);
        boolean Running=true;//this is a flag
        while(Running){
            System.out.println("-".repeat(tableWidth));
            System.out.print("\n");
            System.out.println("Input '1' to add book(s)");
            System.out.println("Input '2' to remove book(s)");
            System.out.println("Input '3' to sort book(s)");
            System.out.println("Input '4' to search book(s)");
            System.out.println("Input '5' to show statistics of book(s)");
            System.out.println("Input '6' to borrow a book from the shelf");
            System.out.println("Input 'quit' to deactivate the management system");
            System.out.print("User input: ");
            String usrInput=input.nextLine().trim();//no need for type safe because we are reading Line
            switch(usrInput){ //using switch case instead of if else if because the logic here fits better
                case "1":
                    addBooks();
                    break;
                case "2":
                    removeBooks();
                    break;
                case "3":
                    sortBooks();
                    break;
                case "4":
                    searchBooks();
                    break;
                case "5":
                    showStatistics();
                    break;
                case "6":
                    borrowBook();
                    break;
                case "quit":
                    System.out.println("Are you sure you want to quit the program? Your data will not be saved. (yes/no)");
                    if(input.nextLine().equalsIgnoreCase("yes")){
                        System.out.println("Thank you for using the Library Management System. Have a nice day.");
                        Running=false;
                        break;    
                    }
                    break;
                default:
                    System.out.println("Invalid input, try again");
                    break;
            }
        }
    }
}
