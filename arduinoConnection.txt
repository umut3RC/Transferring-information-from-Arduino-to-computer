package com.company;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.InputEvent;
import java.awt.event.KeyEvent;
import java.util.Scanner;
import javax.swing.*;
import javax.swing.border.Border;

import com.fazecast.jSerialComm.SerialPort;// serial connection to Arduino

import java.io.PrintWriter;
//mouse


public class Main {
    static SerialPort chosenPort;

    public static void main(String[] args) throws AWTException {
        JFrame window = new JFrame();
        window.setTitle("Arduino connection. By umter_StdS");
        window.setSize(650, 350);
        window.setLayout(new BorderLayout());
        window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // create a drop-down box and connect button, then place them at the top of the window
        JComboBox<String> portList = new JComboBox<String>();
        JButton connectButton = new JButton("Connect");
        //JButton clearButton = new JButton("Clear Msg");
        //JButton ledOn = new JButton("LED On");
        //JButton sendMsg = new JButton("Send");
        JCheckBox cbox1 = new JCheckBox("1");
        JCheckBox cbox2 = new JCheckBox("2");
        JCheckBox cbox3 = new JCheckBox("3");
        JCheckBox cbox4 = new JCheckBox("4");
        JCheckBox cbox5 = new JCheckBox("5");
        JCheckBox cbox6 = new JCheckBox("6");
        JCheckBox cbox7 = new JCheckBox("7");
        JCheckBox cbox8 = new JCheckBox("8");
        JCheckBox cbox9 = new JCheckBox("9");
        JCheckBox cbox10 = new JCheckBox("10");

        JTextField in1 = new JTextField("10000");//shift
        JTextField in2 = new JTextField("10001");//control
        JTextField in3 = new JTextField("10002");//q
        JTextField in4 = new JTextField("10003");//e
        JTextField in5 = new JTextField("10004");//g
        JTextField in6 = new JTextField("10005");//tab
        JTextField in7 = new JTextField("10006");//r
        JTextField in8 = new JTextField("10007");//space
        JTextField in9 = new JTextField("20000");//mouse
        JTextField in10 = new JTextField("5000");//stop

        JTextField out1 = new JTextField("16000");
        JTextField out2 = new JTextField("17000");
        JTextField out3 = new JTextField("81000");
        JTextField out4 = new JTextField("69000");
        JTextField out5 = new JTextField("71000");
        JTextField out6 = new JTextField("9000");
        JTextField out7 = new JTextField("82000");
        JTextField out8 = new JTextField("32000");
        JTextField out9 = new JTextField("mouse");
        JTextField out10 = new JTextField("5000");

        JLabel l_in = new JLabel("In");
        JLabel l_out = new JLabel("Out");


        in1.setPreferredSize(new Dimension(50 ,20));
        out1.setPreferredSize(new Dimension(50 ,20));
        in2.setPreferredSize(new Dimension(50 ,20));
        out2.setPreferredSize(new Dimension(50 ,20));
        in3.setPreferredSize(new Dimension(50 ,20));
        out3.setPreferredSize(new Dimension(50 ,20));
        in4.setPreferredSize(new Dimension(50 ,20));
        out4.setPreferredSize(new Dimension(50 ,20));

        in5.setPreferredSize(new Dimension(50 ,20));
        out5.setPreferredSize(new Dimension(50 ,20));
        in6.setPreferredSize(new Dimension(50 ,20));
        out6.setPreferredSize(new Dimension(50 ,20));
        in7.setPreferredSize(new Dimension(50 ,20));
        out7.setPreferredSize(new Dimension(50 ,20));
        in8.setPreferredSize(new Dimension(50 ,20));
        out8.setPreferredSize(new Dimension(50 ,20));
        in9.setPreferredSize(new Dimension(50 ,20));
        out9.setPreferredSize(new Dimension(50 ,20));
        in10.setPreferredSize(new Dimension(50 ,20));
        out10.setPreferredSize(new Dimension(50 ,20));


        JLabel label1 = new JLabel();
        label1.setText("Input data");

        JPanel panel_port = new JPanel();
        JPanel p_in = new JPanel();
        JPanel p_out = new JPanel();
        JPanel p_cbox = new JPanel();

        p_in.add(l_in);
        p_in.add(in1);
        p_in.add(in2);
        p_in.add(in3);
        p_in.add(in4);
        p_in.add(in5);
        p_in.add(in6);
        p_in.add(in7);
        p_in.add(in8);
        p_in.add(in9);
        p_in.add(in10);

        p_out.add(l_out);
        p_out.add(out1);
        p_out.add(out2);
        p_out.add(out3);
        p_out.add(out4);
        p_out.add(out5);
        p_out.add(out6);
        p_out.add(out7);
        p_out.add(out8);
        p_out.add(out9);
        p_out.add(out10);

        panel_port.add(cbox1);
        panel_port.add(cbox2);
        panel_port.add(cbox3);
        panel_port.add(cbox4);
        panel_port.add(cbox5);
        panel_port.add(cbox6);
        panel_port.add(cbox7);
        panel_port.add(cbox8);
        panel_port.add(cbox9);
        panel_port.add(cbox10);

        //field1.setVisible(true);
        //field1.setColumns(20);
        //topPanel.add(sendMsg);
        //topPanel.add(clearButton);
        panel_port.add(portList);
        panel_port.add(connectButton);


        //JSlider slider = new JSlider();
        //slider.setMaximum(1023);
        //window.add(slider);

        //p_data.setLayout(new BoxLayout(p_data, BoxLayout.PAGE_AXIS));
        //p_cbox.setComponentOrientation(ComponentOrientation.LEFT_TO_RIGHT);
        JCheckBox boxes[] = {cbox1,cbox2,cbox3,cbox4,cbox5,cbox6,cbox7,cbox8,cbox9,cbox10};
        JTextField outbox[] = {out1,out2,out3,out4,out5,out6,out7,out8,out9,out10};

        for (int i = 0; i < boxes.length; i++)
        {
            boxes[i].setSelected(true);
        }

        window.add(p_cbox, BorderLayout.PAGE_START);
        window.add(panel_port , BorderLayout.PAGE_END);
        window.add(p_in, BorderLayout.PAGE_START);
        window.add(p_out, BorderLayout.CENTER);


        SerialPort[] portNames = SerialPort.getCommPorts();// get the list of serial ports
        for (int i = 0; i < portNames.length; i++) {
            portList.addItem(portNames[i].getSystemPortName());
        }
        ///////////////////
        connectButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {

                if (connectButton.getText().equals("Connect")) {
                    // attempt to connect to the serial port
                    chosenPort = SerialPort.getCommPort(portList.getSelectedItem().toString());
                    chosenPort.setComPortTimeouts(SerialPort.TIMEOUT_SCANNER, 0, 0);
                    if (chosenPort.openPort()) {
                        connectButton.setText("Disconnect");
                        portList.setEnabled(false);
                    }

                    // create a new thread that listens for incoming text
                    Thread thread = new Thread() {
                        @Override
                        public void run() {
                            Scanner scanner = new Scanner(chosenPort.getInputStream());
                            while (scanner.hasNextLine()) {
                                try {
                                    // read a line of data from the serial port
                                    String line = scanner.nextLine();
                                    //  System.out.println(line);
                                    int intline = Integer.parseInt(line);
                                    int getCode;
                                    double dx = MouseInfo.getPointerInfo().getLocation().getX();
                                    double dy = MouseInfo.getPointerInfo().getLocation().getY();
                                    int x = (int) dx;
                                    int y = (int) dy;

                                    Robot r = new Robot();

                                    if (intline < -1 && intline < 2000)
                                    {
                                        r.mouseMove((intline * -1), y);
                                    }
                                    else if (intline > -1 && intline < 2000)
                                    {
                                        r.mouseMove(x, intline);
                                    }
                                    if(line.equals("20000"))
                                    {
                                        r.mousePress(InputEvent.BUTTON1_DOWN_MASK);
                                        r.mouseRelease(InputEvent.BUTTON1_DOWN_MASK);
                                    }
                                    if (intline == 5000)
                                    {
                                        for ( int i = 0; i < boxes.length; i++)
                                        {
                                            if(!boxes[i].isSelected())
                                            {
                                                getCode = (Integer.parseInt(outbox[i].getText())) / 1000;
                                               r.keyRelease(getCode);
                                            }
                                        }
                                    }
                                    if (line.equals(in1.getText()))
                                    {
                                        getCode = (Integer.parseInt(out1.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox1.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in2.getText()))
                                    {
                                        getCode = (Integer.parseInt(out2.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox2.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in3.getText()))
                                    {
                                        getCode = (Integer.parseInt(out3.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox3.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in4.getText()))
                                    {
                                        getCode = (Integer.parseInt(out4.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox4.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in5.getText()))
                                    {
                                        getCode = (Integer.parseInt(out5.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox5.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in6.getText()))
                                    {
                                        getCode = (Integer.parseInt(out6.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox6.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in7.getText()))
                                    {
                                        getCode = (Integer.parseInt(out7.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox7.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in8.getText()))
                                    {
                                        getCode = (Integer.parseInt(out8.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox8.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in9.getText()))
                                    {
                                        getCode = (Integer.parseInt(out9.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox9.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    else if (line.equals(in10.getText()))
                                    {
                                        getCode = (Integer.parseInt(out10.getText())) / 1000;
                                        r.keyPress(getCode);
                                        if (cbox10.isSelected())
                                        {
                                            r.keyRelease(getCode);
                                        }
                                    }
                                    window.repaint();

                                } catch (Exception e) {
                                }
                            }
                            scanner.close();
                        }
                    };
                    thread.start();
                } else {
                    // disconnect from the serial port
                    chosenPort.closePort();
                    portList.setEnabled(true);
                    connectButton.setText("Connect");

                }
            }

        });
        // show the window
        window.setVisible(true);
    }
}