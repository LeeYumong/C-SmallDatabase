#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <sstream>

// Class to handle documents in our database
class Document {
public:
    std::string id;       // Unique ID for each document
    std::string title;    // Title of the document
    std::string content;  // Content of the document

    // Constructor to easily create a document
    Document(const std::string& id, const std::string& title, const std::string& content)
        : id(id), title(title), content(content) {}

    // Convert the document into a string for saving to the file
    std::string to_string() const {
        return id + "," + title + "," + content;
    }
};

// Class to manage the document database
class Database {
private:
    std::string filename;  // File where the documents are stored

    // Split a string by a delimiter (for parsing file lines)
    std::vector<std::string> split(const std::string& s, char delimiter) {
        std::vector<std::string> tokens;
        std::string token;
        std::istringstream tokenStream(s);
        while (std::getline(tokenStream, token, delimiter)) {
            tokens.push_back(token);
        }
        return tokens;
    }

public:
    // Initialize the database with a filename
    Database(const std::string& filename) : filename(filename) {}

    // Add a new document to the database
    void addDocument(const Document& document) {
        std::ofstream file(filename, std::ios::app);
        if (file.is_open()) {
            file << document.to_string() << "\n";
            file.close();
            std::cout << "Document added successfully!\n";
        } else {
            std::cerr << "Error opening the file.\n";
        }
    }

    // Display all documents from the database
    void viewAllDocuments() {
        std::ifstream file(filename);
        std::string line;
        if (file.is_open()) {
            std::cout << "ID | Title | Content\n";
            while (std::getline(file, line)) {
                std::vector<std::string> document = split(line, ',');
                if (document.size() == 3) {
                    std::cout << document[0] << " | " << document[1] << " | " << document[2] << "\n";
                }
            }
            file.close();
        } else {
            std::cerr << "Error opening the file.\n";
        }
    }

    // Update a document based on its ID
    void updateDocument(const std::string& id, const Document& newDocument) {
        std::ifstream file(filename);
        std::vector<Document> documents;
        std::string line;
        bool found = false;

        if (file.is_open()) {
            while (std::getline(file, line)) {
                std::vector<std::string> documentData = split(line, ',');
                if (documentData.size() == 3) {
                    if (documentData[0] == id) {
                        documents.push_back(newDocument);  // Replace with updated document
                        found = true;
                    } else {
                        documents.push_back(Document(documentData[0], documentData[1], documentData[2]));
                    }
                }
            }
            file.close();

            if (found) {
                std::ofstream outFile(filename, std::ios::trunc);
                for (const auto& doc : documents) {
                    outFile << doc.to_string() << "\n";
                }
                outFile.close();
                std::cout << "Document updated successfully!\n";
            } else {
                std::cerr << "Document with ID " << id << " not found.\n";
            }
        } else {
            std::cerr << "Error opening the file.\n";
        }
    }

    // Remove a document based on its ID
    void deleteDocument(const std::string& id) {
        std::ifstream file(filename);
        std::vector<Document> documents;
        std::string line;
        bool found = false;

        if (file.is_open()) {
            while (std::getline(file, line)) {
                std::vector<std::string> documentData = split(line, ',');
                if (documentData.size() == 3) {
                    if (documentData[0] != id) {
                        documents.push_back(Document(documentData[0], documentData[1], documentData[2]));
                    } else {
                        found = true;
                    }
                }
            }
            file.close();

            if (found) {
                std::ofstream outFile(filename, std::ios::trunc);
                for (const auto& doc : documents) {
                    outFile << doc.to_string() << "\n";
                }
                outFile.close();
                std::cout << "Document deleted successfully!\n";
            } else {
                std::cerr << "Document with ID " << id << " not found.\n";
            }
        } else {
            std::cerr << "Error opening the file.\n";
        }
    }
};

// Function to display a menu with choices for the user
void displayMenu() {
    std::cout << "\nDatabase Menu:\n";
    std::cout << "1. Add Document\n";
    std::cout << "2. View All Documents\n";
    std::cout << "3. Update Document\n";
    std::cout << "4. Delete Document\n";
    std::cout << "5. Exit\n";
    std::cout << "Choose an option: ";
}

int main() {
    Database db("database.txt");  // Create a Database instance with "database.txt"
    int choice;

    do {
        displayMenu();  // Show the menu
        std::cin >> choice;  // Get user choice

        switch (choice) {
        case 1: {
            // Handle adding a new document
            std::string id, title, content;
            std::cout << "Enter ID: ";
            std::cin >> id;
            std::cout << "Enter Title: ";
            std::cin.ignore();  // Ignore leftover newline
            std::getline(std::cin, title);
            std::cout << "Enter Content: ";
            std::getline(std::cin, content);
            db.addDocument(Document(id, title, content));  // Add document
            break;
        }
        case 2:
            // Handle viewing all documents
            db.viewAllDocuments();  // Display documents
            break;
        case 3: {
            // Handle updating an existing document
            std::string id, title, content;
            std::cout << "Enter the ID of the document to update: ";
            std::cin >> id;
            std::cout << "Enter new Title: ";
            std::cin.ignore();
            std::getline(std::cin, title);
            std::cout << "Enter new Content: ";
            std::getline(std::cin, content);
            db.updateDocument(id, Document(id, title, content));  // Update document
            break;
        }
        case 4: {
            // Handle deleting a document
            std::string id;
            std::cout << "Enter the ID of the document to delete: ";
            std::cin >> id;
            db.deleteDocument(id);  // Remove document
            break;
        }
        case 5:
            // Exit the program
            std::cout << "Exiting...\n";
            break;
        default:
            std::cout << "Invalid option. Please try again.\n";
            break;
        }
    } while (choice != 5);  // Repeat until user chooses to exit

    return 0;
}
