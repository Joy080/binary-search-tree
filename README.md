#include <stdio.h>
#include <stdlib.h>

// Define a structure for a binary search tree node
struct TreeNode {
    int data;
    struct TreeNode *left;
    struct TreeNode *right;
};

// Function to create a new node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    return newNode;
}

// Function to insert a node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int data) {
    if (root == NULL) {
        return createNode(data);
    }
    if (data < root->data) {
        root->left = insertNode(root->left, data);
    } else if (data > root->data) {
        root->right = insertNode(root->right, data);
    }
    return root;
}

// Function to delete a node from the BST
struct TreeNode* deleteNode(struct TreeNode* root, int key) {
    if (root == NULL) return root;
    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    } else {
        // Node with only one child or no child
        if (root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }
        // Node with two children: Get the inorder successor (smallest in the right subtree)
        struct TreeNode* temp = root->right;
        while (temp && temp->left != NULL) {
            temp = temp->left;
        }
        // Copy the inorder successor's content to this node
        root->data = temp->data;
        // Delete the inorder successor
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

// Function to calculate the height of the BST
int height(struct TreeNode* root) {
    if (root == NULL) return -1;
    int leftHeight = height(root->left);
    int rightHeight = height(root->right);
    return (leftHeight > rightHeight) ? (leftHeight + 1) : (rightHeight + 1);
}

// Function to find the level of a node in the BST
int findLevel(struct TreeNode* root, int key, int level) {
    if (root == NULL) return -1;
    if (root->data == key) return level;
    int leftLevel = findLevel(root->left, key, level + 1);
    if (leftLevel != -1) return leftLevel;
    return findLevel(root->right, key, level + 1);
}

// Function to print the level and height of a node in the BST
void printLevelAndHeight(struct TreeNode* root, int key) {
    int level = findLevel(root, key, 0);
    int h = height(root);
    if (level != -1)
        printf("Level of %d is %d and height of %d is %d\n", key, level, key, h);
    else
        printf("Node with data %d not found in the BST\n", key);
}

// Function to print the elements of the BST in inorder traversal
void inorderTraversal(struct TreeNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->data);
        inorderTraversal(root->right);
    }
}

int main() {
    int arr[] = {30, 20, 40, 10, 25, 35, 45, 5, 15};
    int n = sizeof(arr) / sizeof(arr[0]);

    // Create the BST from the array
    struct TreeNode* root = NULL;
    for (int i = 0; i < n; i++) {
        root = insertNode(root, arr[i]);
    }

    printf("Inorder traversal of the BST: ");
    inorderTraversal(root);
    printf("\n");

    // Delete a node (e.g., delete node with value 20)
    int keyToDelete = 20;
    root = deleteNode(root, keyToDelete);
    printf("BST after deleting %d: ", keyToDelete);
    inorderTraversal(root);
    printf("\n");

    // Print the height of the BST
    printf("Height of the BST: %d\n", height(root));

    // Print the level and height of any node in the BST
    int nodeToFind = 10;
    printLevelAndHeight(root, nodeToFind);

    return 0;
}
