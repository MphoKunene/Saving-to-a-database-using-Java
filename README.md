# Bookstore-using-a-database-in-Java
//using a bookstore as an example
Using a database in java

import java.util.Scanner;
import java.sql.*;
 
public class Bookstore {
 
    public static void main(String[] args) {
        try(
                //connect to database
                Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/ebookstore?useSSL=false","root","Mk0627481092");
                Statement stmt = conn.createStatement();
                )
        {
            //Scanner object
            Scanner scan = new Scanner(System.in);
             
            System.out.println("Book Store options:\n1)Press 1 to Enter new Book \n2)Press 2 to update existing Book \n3)Press 3 to Delete a book"
                    + "\n4)Press 4 to Search for a book \n5)Press 5 to Exit Bookstore");
            int options = scan.nextInt();
             
            //if arugement is true carry out body
            if(options == 1) {
                System.out.println("You have Selected Enter new Book");
                System.out.println("Please input Book id");
                int id = scan.nextInt();
                 
                scan.nextLine();
                 
                System.out.println("Please enter the title of the book");
                String title = scan.nextLine();
                 
                 
                System.out.println("Please input the name of the Author");
                String author = scan.nextLine();
                 
                System.out.println("Please input Bood Quantity");
                int qty = scan.nextInt();
                 
                ////sql query to insert
                String sql = "Insert into books " + " (id, Title, Author, Qty)" + " values (?, ?, ?, ?)";
                PreparedStatement ps = conn.prepareStatement(sql);
                ps.setInt(1, id);
                ps.setString(2, title);
                ps.setString(3, author);
                ps.setInt(4, qty);
                 
                //carries out the update
                ps.executeUpdate();
                System.out.println("Book recorded on the database");
            }
            //else this statement is true then carry out body
            else if(options == 2) {
                System.out.println("You have Selected Update Existing book");
                System.out.println("Press 1 to update id \nPress 2 to update title \nPress 3 to update Author \nPress 4 to update Qty");
                int optionUpdate = scan.nextInt();
                 
                //update preparestatement
                if(optionUpdate == 1) {
                    System.out.println("Please input id record you want to update");
                    int existid = scan.nextInt();
                     
                    System.out.println("Please input new id to update");
                    int updateid = scan.nextInt();
                     
                    //sql query to update
                    String query1 = "update books " + "set id = '" + updateid + "' where id = (" + existid + ")";
                    PreparedStatement ps = conn.prepareStatement(query1);
                    ps.executeUpdate(query1);
                     
                    System.out.println("Id updated");
                     
                }
                 
                else if(optionUpdate == 2) {
                     
                     
                     
                    System.out.println("Please input id record you want to update");
                    int existid = scan.nextInt();
                     
                    System.out.println("Please input new title to update");
                    String updatedTitle = scan.nextLine();
                     
                     
                    String query2 = "update books set Title = '" + updatedTitle + "' where id = (" + existid + ")";
                    PreparedStatement ps = conn.prepareStatement(query2);
                    ps.executeUpdate(query2);
                     
                    System.out.println("title updated");
                     
                }
                 
                else if(optionUpdate == 3) {
                    scan.nextLine();
                     
                    System.out.println("Please input id record you want to update");
                    int existid = scan.nextInt();
                     
                    System.out.println("Please input new author to update");
                    String updateAuthor = scan.nextLine();
                     
                     
                    String query3 = "update books set Author = '" + updateAuthor + "' where id = (" + existid + ")";
                    PreparedStatement ps = conn.prepareStatement(query3);
                    ps.executeUpdate(query3);
                     
                    System.out.println("Author updated");
                }
                 
                else  {
                    System.out.println("You have selected update Qty of a book");
                     
                    scan.nextLine();
                     
                    System.out.println("Please input id record you want to update");
                    int existid = scan.nextInt();
                     
                    System.out.println("Please input new qty to update");
                    int updateQty = scan.nextInt();
                     
                     
                    String query3 = "update books set Qty = '" + updateQty + "' where id = (" + existid + ")";
                    PreparedStatement ps = conn.prepareStatement(query3);
                    ps.executeUpdate(query3);
                     
                    System.out.println("Qty updated");
                }
                 
                 
            }
            else if(options == 3) {
                System.out.println("You have selected Delete a Book");
                 
                System.out.println("Please input id record you want to delete");
                int existid = scan.nextInt();
                 
                //sql query to delete
                String queryDelete = "delete from books where id = '" + existid + "'";
                PreparedStatement ps = conn.prepareStatement(queryDelete);
                ps.executeUpdate(queryDelete);
                 
                System.out.println("Book record deleted");
            }
            else if(options == 4) {
                System.out.println("You have selected Search for a book");
                 
                System.out.println("Please input id record you want to search");
                int existid = scan.nextInt();
                 
                //sql query to dearch
                String querySearch = "Select * from books where id = '" + existid + "'";
                ResultSet rs = stmt.executeQuery(querySearch);
                 
                // Move the cursor to the next row, return false if no more row
                if(rs.next()){
                    //print output once
                    do {
                        //print database details on console
                        System.out.println(rs.getInt("id") + ", " + rs.getString("Title") + ", "
                                + rs.getString("Author") + ", " + rs.getInt("Qty"));
                        //while this statement is true it will go to do
                    }while(rs.next());
                }
                else {
                    System.out.println("Record not found");
                }
                 
                 
            }
            else {
                //terminates program
                System.out.print("System Exited");
                System.exit(0);
            }
        }
        catch(SQLException ex) {
            ex.printStackTrace ();
        }
         
     
    }
 
}
