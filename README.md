# Smart-Warehouse-Grid-Locator-
Smart Warehouse Grid Locator project in Java using Object-Oriented Programming and 2D arrays.  Implements item storage, search by ID, and precise location tracking within a simulated warehouse grid.

GridItem.java

public class GridItem {
    private String itemId;
    private String itemName;
    private int quantity;
    public GridItem(String itemId, String itemName, int quantity) {
        this.itemId   = itemId;
        this.itemName = itemName;
        this.quantity = quantity;
    }
    public String getItemId()   { return itemId; }
    public String getItemName() { return itemName; }
    public int getQuantity()    { return quantity; }
    public void setItemId(String itemId)     { this.itemId = itemId; }
    public void setItemName(String itemName) { this.itemName = itemName; }
    public void setQuantity(int quantity)    { this.quantity = quantity; }
    @Override
    public String toString() {
        return "GridItem { ID: " + itemId + ", Name: " + itemName + ", Quantity: " + quantity + " }";
    }
}

Warehouse.java

public class Warehouse {
    private GridItem[][] grid;
    private int rows;
    private int cols;
    public Warehouse(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;
        this.grid = new GridItem[rows][cols];
    }
    public void addItem(int row, int col, GridItem item) {
        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            System.out.println("Error: Position [" + row + ", " + col + "] is out of bounds.");
            return;
        }
        if (grid[row][col] != null) {
            System.out.println("Error: Cell [" + row + ", " + col + "] is already occupied by "
                    + grid[row][col].getItemId() + ".");
            return;
        }
        grid[row][col] = item;
    }
    public void removeItem(int row, int col) {
        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            System.out.println("Error: Position [" + row + ", " + col + "] is out of bounds.");
            return;
        }
        if (grid[row][col] == null) {
            System.out.println("Error: Cell [" + row + ", " + col + "] is already empty.");
            return;
        }
        System.out.println("Removed: " + grid[row][col] + " from [" + row + ", " + col + "].");
        grid[row][col] = null;
    }
    public void searchItem(String itemId) {
        System.out.println("Searching for item ID: " + itemId);
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] != null && grid[r][c].getItemId().equals(itemId)) {
                    System.out.println("Item found at Row: " + r + ", Column: " + c);
                    System.out.println("Details: " + grid[r][c]);
                    System.out.println();
                    return;
                }
            }
        }
        System.out.println("Item not found in warehouse");
        System.out.println();
    }
    public void displayGrid() {
        System.out.println("\n===== Warehouse Grid (" + rows + "x" + cols + ") =====");
        for (int r = 0; r < rows; r++) {
            System.out.print("Row " + r + ": ");
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] != null) {
                    System.out.printf("| %-22s ", grid[r][c].getItemId() + " (" + grid[r][c].getItemName() + ")");
                } else {
                    System.out.printf("| %-22s ", "[ EMPTY ]");
                }
            }
            System.out.println("|");
        }
        System.out.println("==========================================\n");
    }
    public int getTotalItems() {
        int count = 0;
        for (int r = 0; r < rows; r++)
            for (int c = 0; c < cols; c++)
                if (grid[r][c] != null) count++;
        return count;
    }
    public int getEmptySlots() {
        return (rows * cols) - getTotalItems();
    }
}

Main.java

public class Main {
    public static void main(String[] args) {
        // Step 1: Create the Warehouse (5x5 grid)
        Warehouse warehouse = new Warehouse(5, 5);
        // Step 2: Populate the grid with GridItem objects
        warehouse.addItem(0, 0, new GridItem("I101", "Laptop",     10));
        warehouse.addItem(1, 2, new GridItem("I102", "Phone",      25));
        warehouse.addItem(2, 4, new GridItem("I103", "Tablet",      8));
        warehouse.addItem(3, 1, new GridItem("I104", "Headphones", 50));
        warehouse.addItem(4, 3, new GridItem("I105", "Charger",   100));
        // Step 3: Display the full grid
        warehouse.displayGrid();
        System.out.println("Total Items : " + warehouse.getTotalItems());
        System.out.println("Empty Slots : " + warehouse.getEmptySlots());
        System.out.println();
        // Step 4: Search for items by ID
        warehouse.searchItem("I102");  // Found at Row 1, Column 2
        warehouse.searchItem("I999");  // Not found
        // Step 5: Add a new item
        System.out.println("--- Adding I106 (Monitor) at [2][1] ---");
        warehouse.addItem(2, 1, new GridItem("I106", "Monitor", 5));
        warehouse.displayGrid();
        // Step 6: Remove an item
        System.out.println("--- Removing item at [1][2] ---");
        warehouse.removeItem(1, 2);
        warehouse.displayGrid();
        // Edge cases
        System.out.println("--- Adding to occupied slot [0][0] ---");
        warehouse.addItem(0, 0, new GridItem("I107", "Camera", 3));
        System.out.println("--- Removing from empty slot [0][1] ---");
        warehouse.removeItem(0, 1);
    }
}








