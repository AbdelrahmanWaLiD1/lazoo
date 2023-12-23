# lazoo
My first java project

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;
import java.util.Scanner;
import javax.swing.JOptionPane;
public class Main {
    public static void main(String[] args) throws ParseException {
        ShoppingCart cart = new ShoppingCart();
        Scanner s = new Scanner(System.in);
        Item[] item = new Item[100];
                int itemsCount=0;
        while(true) {
            System.out.println("enter (C) for customer or (E) for employee or (press any key) to exit");
            String checkOption = s.next();
            if (checkOption.equalsIgnoreCase("E")) {
                System.out.println("how many items do you have in the inventory?");
                 itemsCount = Integer.parseInt(s.next());
                MenuMaker(item, itemsCount, s);
            } else if (checkOption.equalsIgnoreCase("C")) {
                customer(s, item, cart, itemsCount);
            } else {
                break;
            }
        }
    }
        public static void MenuMaker(Item[] item, int itemsCount, Scanner s) {
                Employee employee = new Employee();
                System.out.println("enter your name");
                String name=s.next();
                employee.setName(name);
                System.out.println("enter your age");
                int age=Integer.parseInt(s.next());
                employee.setAge(age);
                System.out.println("enter your id");
                String id=s.next();
                employee.setId(id);
                for (int i = 0; i < itemsCount; i++) {
                    String itemName = JOptionPane.showInputDialog("Enter item name");
                    String itemPrice = JOptionPane.showInputDialog("Enter price");
                    String itemQuantity = JOptionPane.showInputDialog("Enter quantity");
                    item[i] = new Item(itemName, Integer.parseInt(itemQuantity), Double.parseDouble(itemPrice), itemName + Math.random());
                }
        }
        public static void customer(Scanner s,Item [] item,ShoppingCart cart,int itemsCount) throws ParseException {
        Customer customer=new Customer();
            System.out.println("enter your name");
            String name=s.next();
            customer.setName(name);
            System.out.println("enter your age");
            int age=Integer.parseInt(s.next());
            customer.setAge(age);
            System.out.println("choose from the menu using item's name:");
            for (int i = 0; i < itemsCount ; i++) {
                System.out.println("item"+(i+1)+":"+item[i]);
            }
            String cont="ok";
            String itemName;
            int itemQuantity;
            for (int j = 0; j<10&&cont.equalsIgnoreCase("Ok") ; j++) {
                for (int i = 0; i < item.length; i++) {
                    itemName = s.next();
                    if (item[i].getName().equalsIgnoreCase(itemName)) {
                        System.out.println("enter the quantity:");
                        itemQuantity=Integer.parseInt(s.next());
                        cart.add(item[i],itemQuantity);
                    } else {
                        System.out.println("item not found");
                        i--;
                    }
                    System.out.println("Enter (ok) to add another item or (no) to see your cart");
                    cont = s.next();
                        if(cont.equalsIgnoreCase("no")){
                            break;
                    }
                }
                }
                cart.printCart();
                System.out.println("Total=" + cart.getTotalAmount());
                System.out.println("do you want to remove item?,enter (Y) to remove or (N) ");
                while (true){
                    String check = s.next();
                if (check.equalsIgnoreCase("y")) {
                    itemName = s.next();
                    cart.remove(itemName);
                    cart.printCart();
                    break;
                } else if (check.equalsIgnoreCase("n")) {
                    break;
                } else {
                    System.out.println("invalid input try again");
                }
            }
                while (true) {
                   String paymentMethod = JOptionPane.showInputDialog("Payment method (CreditCard/PayPal)");
                    if (paymentMethod.equalsIgnoreCase("creditCard") || paymentMethod.equalsIgnoreCase("PayPal")) {
                        paying(paymentMethod, s, cart);
                        break;
                    } else {
                        System.out.println("invalid input try again");
                    }
                }
            }

    public static void paying(String paymentMethod,Scanner s,ShoppingCart cart) throws ParseException {
        if (paymentMethod.equalsIgnoreCase("CreditCard")) {
            System.out.println("enter the credit information");
            System.out.println("Name");
            String name = s.next();
            System.out.println("Card Number");
            String cardNumber = s.next();
            System.out.println("CVV");
            String cvv = s.next();
            System.out.println("Expiry date month");
            String month = s.next();
            System.out.println("Expiry date year");
            String year = s.next();
            String expDate = month + "-" + year;
            SimpleDateFormat formatter = new SimpleDateFormat("MM-yy", Locale.ENGLISH);
            Date date = formatter.parse(expDate);
            CreditCard c = new CreditCard(name, cardNumber, cvv, date);
            if (c.isValid()) {
                cart.checkout(c);
            } else {
                System.out.println("Credit Card is expired ");
            }
        } else if (paymentMethod.equalsIgnoreCase("paypal")) {
            System.out.println("enter the email");
            String mail = s.next();
            System.out.println("enter the password");
            String pass = s.next();
            PayPal payPal = new PayPal(mail, pass);
            while(true) {
                if (payPal.isValid()) {
                    cart.checkout(payPal);
                    break;
                }else {
                    System.out.println("invalid mail or password ,try again");
                    mail = s.next();
                    System.out.println("enter the password");
                    pass = s.next();
                    payPal.setEmail(mail);
                    payPal.setPassword(pass);
                }
            }
        }
    }
}

