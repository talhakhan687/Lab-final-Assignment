# Lab-final-Assignment

import java.util.*;

class Doc {
    private String nm;
    private String spec;

    public Doc(String nm, String spec) {
        this.nm = nm;
        this.spec = spec;
    }

    public String getNm() {
        return nm;
    }

    public String getSpec() {
        return spec;
    }

    public void showAvail() {
        System.out.println("Availability: General.");
    }
}

class Asst extends Doc {
    public Asst(String nm) {
        super(nm, "Assistant");
    }

    @Override
    public void showAvail() {
        System.out.println("Available physically.");
    }
}

class Spec extends Doc {
    public Spec(String nm) {
        super(nm, "Specialist");
    }

    @Override
    public void showAvail() {
        System.out.println("Requires confirmation.");
    }
}

class Ptnt {
    private static int ptntID = 1;
    private int id;
    private String nm;
    private int age;

    public Ptnt(int id, String nm, int age) {
        this.id = id;
        this.nm = nm;
        this.age = age;
    }

    public int getID() {
        return id;
    }

    public String getNm() {
        return nm;
    }

    public int getAge() {
        return age;
    }
}

class Apt {
    private Doc d;
    private Ptnt p;
    private String date;
    private String time;

    public Apt(Doc d, Ptnt p, String date, String time) {
        this.d = d;
        this.p = p;
        this.date = date;
        this.time = time;
    }

    @Override
    public String toString() {
        return "Apt [Doctor: " + d.getNm() + ", Spec: " + d.getSpec() + ", Ptnt: " + p.getNm() +
                " (ID: " + p.getID() + "), Date: " + date + ", Time: " + time + "]";
    }
}

public class AptMgtSys {
    private static List<Doc> docs = new ArrayList<>();
    private static List<Apt> apts = new ArrayList<>();

    public static void main(String[] args) {
        docs.add(new Asst("Dr. Kashem"));
        docs.add(new Spec("Dr. Abul"));

        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n1. Registration Patient");
            System.out.println("2. View Doctors");
            System.out.println("3. Book Appointment");
            System.out.println("4. View doctors Schedule");
            System.out.println("5. Exit");
            System.out.print("Choice: ");
            int ch = sc.nextInt();
            sc.nextLine();

            switch (ch) {
                case 1:
                    regPtnt(sc);
                    break;
                case 2:
                    viewDocs();
                    break;
                case 3:
                    bookApt(sc);
                    break;
                case 4:
                    viewSchedule(sc);
                    break;
                case 5:
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        }
    }

    private static void regPtnt(Scanner sc) {
        System.out.print("Enter Patient ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        System.out.print("Enter Patient Name: ");
        String nm = sc.nextLine();
        System.out.print("Enter Age: ");
        int age = sc.nextInt();
        sc.nextLine();
        Ptnt p = new Ptnt(id, nm, age);
        System.out.println("Registered: " + p.getNm() + " (ID: " + p.getID() + ", Age: " + age + ")");
    }

    private static void viewDocs() {
        for (Doc d : docs) {
            System.out.println("Doc: " + d.getNm() + ", Spec: " + d.getSpec());
            d.showAvail();
        }
    }

    private static void bookApt(Scanner sc) {
        System.out.print("Enter Patient ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        System.out.print("Enter Patient Name: ");
        String nm = sc.nextLine();
        System.out.print("Enter Age: ");
        int age = sc.nextInt();
        sc.nextLine();

        Ptnt p = new Ptnt(id, nm, age);

        System.out.println("Docs:");
        for (int i = 0; i < docs.size(); i++) {
            System.out.println((i + 1) + ". " + docs.get(i).getNm() + " - " + docs.get(i).getSpec());
        }

        System.out.print("Select Doctors number: ");
        int dIdx = sc.nextInt() - 1;
        sc.nextLine();

        if (dIdx < 0 || dIdx >= docs.size()) {
            System.out.println("Invalid doc!");
            return;
        }

        Doc d = docs.get(dIdx);

        System.out.print("Enter Date (DD-MM-YY): ");
        String date = sc.nextLine();
        System.out.print("Enter Time (HH:MM AM/PM): ");
        String time = sc.nextLine();

        Apt apt = new Apt(d, p, date, time);
        apts.add(apt);
        System.out.println("Appointment Booked: " + apt);
    }

    private static void viewSchedule(Scanner sc) {
        System.out.println("Doctor schedule:");
        for (int i = 0; i < docs.size(); i++) {
            System.out.println((i + 1) + ". " + docs.get(i).getNm());
        }

        System.out.print("Select Doctors number: ");
        int dIdx = sc.nextInt() - 1;
        sc.nextLine();

        if (dIdx < 0 || dIdx >= docs.size()) {
            System.out.println("Invalid doctor!");
            return;
        }

        Doc d = docs.get(dIdx);
        System.out.println("Appointments for " + d.getNm() + ":");
        for (Apt apt : apts) {
            if (apt.toString().contains(d.getNm())) {
                System.out.println(apt);
            }
        }
    }
}
