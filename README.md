#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Đinh nghia cau truc cho mot mat hang
struct Item {
    char name[50];
    int quantity;
};

// Đinh nghia cau truc cho mot muc trong danh sach lien ket
struct Node {
    struct Item data;
    struct Node* next;
};

// Ham tao mot muc moi trong danh sach lien ket
struct Node* createNode(struct Item item) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = item;
    newNode->next = NULL;
    return newNode;
}

// Ham them mot mat hang vao danh sach lien ket
void addItem(struct Node** head, struct Item item) {
    if (*head == NULL) {
        *head = createNode(item);
        return;
    }

    struct Node* currNode = *head;
    while (currNode->next != NULL) {
        currNode = currNode->next;
    }

    currNode->next = createNode(item);
}

// Ham hien thi danh sách các mat hang
void displayItems(struct Node* head) {
    if (head == NULL) {
        printf("Danh sach trong.\n");
        return;
    }
    struct Node* currNode = head;
    while (currNode != NULL) {
        printf("Ten mat hang: %s, So luong: %d\n", currNode->data.name, currNode->data.quantity);
        if(currNode->data.quantity <= 2){
                printf("san pham %s sap het, hay nhap them san pham\n",currNode->data.name );}
        currNode = currNode->next;
    }
}

// Hàm tim mot mat hang trong danh sách lien ket theo ten
struct Node* findItem(struct Node* head, char* itemName) {
    struct Node* currNode = head;
    while (currNode != NULL) {
        if (strcmp(currNode->data.name, itemName) == 0) {
            return currNode;
        }
        currNode = currNode->next;
    }
    return NULL;
}

// Ham xoa mot mat hang khoi danh sách lien ket
void deleteItem(struct Node** head, char* itemName) {
    struct Node* currNode = *head;
    struct Node* prevNode = NULL;

    // Truong hop dac biet: neu mat hang can xoa là muc dau tien trong danh sach
    if (currNode != NULL && strcmp(currNode->data.name, itemName) == 0) {
        *head = currNode->next;
        free(currNode);
        printf("Mat hang '%s' da duoc xoa khoi danh sach.\n", itemName);
        return;
    }

    // Tim mat hang can xoa va tim muc truoc no
    while (currNode != NULL && strcmp(currNode->data.name, itemName) != 0) {
        prevNode = currNode;
        currNode = currNode->next;
    }

    // Neu mat hang khong ton tai trong danh sach
    if (currNode == NULL) {
        printf("Mat hang '%s' khong ton tai trong danh sach.\n", itemName);
        return;
    }

    // Xoa mat hang khoi danh sach
    prevNode->next = currNode->next;
    free(currNode);
    printf("Mat hang '%s' da duoc xoa khoi danh sach.\n", itemName);
}

// Hàm cap nhat so luong ton kho cua mot mat hang trong danh sach
void updateItemQuantity(struct Node* head, char* itemName, int newQuantity) {
    struct Node* itemNode = findItem(head, itemName);
    if (itemNode == NULL) {
        printf("Mat hang '%s' khong ton tai trong danh sach.\n", itemName);
        return;
    }

    itemNode->data.quantity = newQuantity;
    printf("So luong ton kho cua mat hang '%s' da duoc cap nhat.\n", itemName);
}
// Hàm tính t?ng s? lư?ng s?n ph?m trong kho
int calculateTotalQuantity(struct Node* head) {
    int totalQuantity = 0;
    struct Node* currNode = head;
    while (currNode != NULL) {
        totalQuantity += currNode->data.quantity;
        currNode = currNode->next;
    }
    return totalQuantity;
}

int main() {
    struct Node* itemList = NULL;

    while (1) {
        printf("\n------ Chuong trinh Quan ly Ban hang ------\n");
        printf("1. Hien thi danh sach mat hang\n");
        printf("2. Them mat hang\n");
        printf("3. Xoa mat hang\n");
        printf("4. Cap nhat so luong ton kho\n");
        printf("5. Hien thi tong so luong san pham trong kho\n");
        printf("6. Thoat\n");
        printf("Vui long chon mot tuy chon: ");

        int choice;
        scanf("%d", &choice);

        switch (choice) {
            case 1: {
                printf("\n------ Danh sách mat hàng ------\n");
                displayItems(itemList);
                break;
            }
            case 2: {
                printf("\n------ Them mat hàng ------\n");
                struct Item newItem;
                printf("Nhap ten mat hang: ");
                scanf("%s", newItem.name);
                printf("Nhap so luong nhap kho: ");
                scanf("%d", &newItem.quantity);
                addItem(&itemList, newItem);
                printf("Mat hang '%s' da duoc them vao danh sach.\n", newItem.name);
                break;
            }
            case 3: {
                printf("\n------ Xoa mat hang ------\n");
                char itemName[50];
                printf("Nhap ten mat hang can xoa: ");
                scanf("%s", itemName);
                deleteItem(&itemList, itemName);
                break;
            }
            case 4: {
                printf("\n------ Cap nhat so luong ton kho ------\n");
                char itemName[50];
                int newQuantity;
                printf("Nhap ten mat hang can cap nhat: ");
                scanf("%s", itemName);
                printf("Nhap so luong ton kho moi: ");
                scanf("%d", &newQuantity);
                updateItemQuantity(itemList, itemName, newQuantity);
                break;
            }
            case 5: {
                 printf("\n------ Tong so luong san pham trong kho ------\n");
                int totalQuantity = calculateTotalQuantity(itemList);
                printf("Tong so luong san pham trong kho: %d\n", totalQuantity);
                break;
            }
            case 6: {
                printf("\nChuong trinh ket thuc.\n");
                exit(0);
            }
            default: {
                printf("\nTuy chon khong hop ly. Vui long thu lai.\n");
                break;
            }
        }
    }
   
    return 0;
}
