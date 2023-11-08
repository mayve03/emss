import java.awt.*;
import java.awt.event.*;
import java.util.LinkedList;
import java.util.Queue;
import javax.swing.*;

public class CovidMonitor extends JFrame {
    private static Queue<Patient> queue = new LinkedList<>();
    private JTextArea textArea;
    private JButton addButton;
    private JButton processButton;

    public CovidMonitor() {
        setTitle("Covid-19 Monitor");
        JLabel titleLabel = new JLabel("Covid-19 Patient Monitoring", SwingConstants.CENTER);
        titleLabel.setFont(titleLabel.getFont().deriveFont(24.0f));
        add(titleLabel, BorderLayout.NORTH);

        JLabel helloLabel = new JLabel("Phinmaed Case Program", SwingConstants.LEFT);
        add(helloLabel, BorderLayout.SOUTH);

        setSize(400, 600);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        textArea = new JTextArea();
        textArea.setEditable(false);
        add(new JScrollPane(textArea), BorderLayout.CENTER);

        addButton = new JButton("Add Patient");
        addButton.addActionListener(new AddButtonListener());
        processButton = new JButton("Process Patient");
        processButton.addActionListener(new ProcessButtonListener());

        JPanel buttonPanel = new JPanel(); // Change Panel to JPanel
        buttonPanel.add(addButton);
        buttonPanel.add(processButton);
        add(buttonPanel, BorderLayout.SOUTH);

        setVisible(true);
    }

    private class AddButtonListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            String firstName = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's first name:");
            String lastName = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's last name:");
            String symptoms = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's symptoms:");
            queue.add(new Patient(firstName, lastName, symptoms));
            textArea.append("Added patient: " + firstName + " " + lastName + "(" + symptoms + ")\");
        }
    }

    private class ProcessButtonListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            if (!queue.isEmpty()) {
                Patient patient = queue.poll();
                textArea.append("Processed patient: " + patient.getFirstName() + " " + patient.getLastName() + "(" + patient.getSymptoms() + ")\");
            } else {
                textArea.append("No patients to process\");
            }
        }
    }

    public static void main(String[] args) {
        CovidMonitor monitor = new CovidMonitor();
    }

    class Patient {
        private String firstName;
        private String lastName;
        private String symptoms;

        public Patient(String firstName, String lastName, String symptoms) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.symptoms = symptoms;
        }

        public String getFirstName() {
            return firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public String getSymptoms() {
            return symptoms;
        }
    }
}
