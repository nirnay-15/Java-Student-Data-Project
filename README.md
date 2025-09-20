import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Course {
    private String courseId;
    private String courseName;
    private int maxCapacity;
    private List<Student> enrolledStudents;

    public Course(String courseId, String courseName, int maxCapacity) {
        this.courseId = courseId;
        this.courseName = courseName;
        this.maxCapacity = maxCapacity;
        this.enrolledStudents = new ArrayList<>();
    }

    public String getCourseId() {
        return courseId;
    }

    public String getCourseName() {
        return courseName;
    }

    public int getMaxCapacity() {
        return maxCapacity;
    }

    public List<Student> getEnrolledStudents() {
        return enrolledStudents;
    }

    public boolean enrollStudent(Student student) {
        if (enrolledStudents.size() < maxCapacity) {
            enrolledStudents.add(student);
            return true;
        }
        return false;
    }

    public void displayCourseDetails() {
        System.out.println("Course ID: " + courseId);
        System.out.println("Course Name: " + courseName);
        System.out.println("Max Capacity: " + maxCapacity);
        System.out.println("Enrolled Students:");
        if (enrolledStudents.isEmpty()) {
            System.out.println("  No students enrolled yet.");
        } else {
            for (Student student : enrolledStudents) {
                System.out.println("  - " + student.getStudentName() + " (ID: " + student.getStudentId() + ")");
            }
        }
    }
}

class Student {
    private String studentId;
    private String studentName;
    private List<Course> enrolledCourses;

    public Student(String studentId, String studentName) {
        this.studentId = studentId;
        this.studentName = studentName;
        this.enrolledCourses = new ArrayList<>();
    }

    public String getStudentId() {
        return studentId;
    }

    public String getStudentName() {
        return studentName;
    }

    public List<Course> getEnrolledCourses() {
        return enrolledCourses;
    }

    public void enrollInCourse(Course course) {
        enrolledCourses.add(course);
    }

    public void displayStudentDetails() {
        System.out.println("Student ID: " + studentId);
        System.out.println("Student Name: " + studentName);
        System.out.println("Enrolled Courses:");
        if (enrolledCourses.isEmpty()) {
            System.out.println("  Not enrolled in any courses yet.");
        } else {
            for (Course course : enrolledCourses) {
                System.out.println("  - " + course.getCourseName() + " (ID: " + course.getCourseId() + ")");
            }
        }
    }
}

public class main {
    private static List<Student> students = new ArrayList<>();
    private static List<Course> courses = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n--- Student Course Management System ---");
            System.out.println("1. Add a new Student");
            System.out.println("2. Add a new Course");
            System.out.println("3. Enroll a Student in a Course");
            System.out.println("4. Display Student Details");
            System.out.println("5. Display Course Details");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1:
                    addStudent();
                    break;
                case 2:
                    addCourse();
                    break;
                case 3:
                    enrollStudentInCourse();
                    break;
                case 4:
                    displayStudentDetails();
                    break;
                case 5:
                    displayCourseDetails();
                    break;
                case 6:
                    System.out.println("Exiting the system. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 6);

        scanner.close();
    }

    private static void addStudent() {
        System.out.print("Enter Student ID: ");
        String id = scanner.nextLine();
        System.out.print("Enter Student Name: ");
        String name = scanner.nextLine();
        Student student = new Student(id, name);
        students.add(student);
        System.out.println("Student added successfully!");
    }

    private static void addCourse() {
        System.out.print("Enter Course ID: ");
        String id = scanner.nextLine();
        System.out.print("Enter Course Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Max Capacity: ");
        int capacity = scanner.nextInt();
        scanner.nextLine();
        Course course = new Course(id, name, capacity);
        courses.add(course);
        System.out.println("Course added successfully!");
    }

    private static void enrollStudentInCourse() {
        System.out.print("Enter Student ID to enroll: ");
        String studentId = scanner.nextLine();
        System.out.print("Enter Course ID to enroll in: ");
        String courseId = scanner.nextLine();

        Student student = findStudentById(studentId);
        Course course = findCourseById(courseId);

        if (student == null) {
            System.out.println("Error: Student not found.");
            return;
        }

        if (course == null) {
            System.out.println("Error: Course not found.");
            return;
        }

        if (course.enrollStudent(student)) {
            student.enrollInCourse(course);
            System.out.println("Student enrolled successfully!");
        } else {
            System.out.println("Error: Course is at full capacity.");
        }
    }

    private static void displayStudentDetails() {
        System.out.print("Enter Student ID to display details: ");
        String studentId = scanner.nextLine();
        Student student = findStudentById(studentId);
        if (student != null) {
            student.displayStudentDetails();
        } else {
            System.out.println("Error: Student not found.");
        }
    }

    private static void displayCourseDetails() {
        System.out.print("Enter Course ID to display details: ");
        String courseId = scanner.nextLine();
        Course course = findCourseById(courseId);
        if (course != null) {
            course.displayCourseDetails();
        } else {
            System.out.println("Error: Course not found.");
        }
    }

    private static Student findStudentById(String id) {
        for (Student student : students) {
            if (student.getStudentId().equals(id)) {
                return student;
            }
        }
        return null;
    }

    private static Course findCourseById(String id) {
        for (Course course : courses) {
            if (course.getCourseId().equals(id)) {
                return course;
            }
        }
        return null;
    }
}
