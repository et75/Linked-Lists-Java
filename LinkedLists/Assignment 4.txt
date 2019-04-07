//======================================================  
// Name       :  Emad Tirmizi
// SID        :  31400222
// Course     :  IT114 
// Section    :  
// Instructor :  Maura Deek
// T.A        :   
//======================================================  
//======================================================  
// Assignment # :  4
// Date         :  11/14/2018
//====================================================== 
//====================================================== 
// Description: We must create a program that gives
// the user the option to search, insert, or delete
// from a linked list. To search the user must input 
// the value they are looking for in the list and the
// system will reply with the index of where its 
// located. If the user selects Insert, the program 
// takes the user’s desired value and desired index
// location. If the index is out of bounds of the list,
// the program will create a popup window saying its 
// out of range. If the user selects delete the system 
// takes the user’s desired index and deletes it from 
// the Linked List. If invalid data is entered, an error
// message will appear in a popup window. 
//====================================================== 

import javafx.scene.layout.GridPane;
import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.geometry.Insets;
import javafx.scene.layout.VBox;
import javafx.scene.layout.BorderPane;
import javafx.scene.control.Label;
import javafx.scene.control.Button;
import javafx.scene.control.TextField;
import javafx.scene.control.TextArea;
import javafx.scene.control.ScrollBar;
import javafx.scene.layout.ColumnConstraints;
import javafx.geometry.Orientation;
import javafx.geometry.Pos;
import javafx.geometry.VPos;
import java.util.*;
import javafx.collections.ObservableList;
import javafx.scene.control.*;
import javafx.collections.FXCollections;

//Create Main Class
public class Main extends Application{	
	//set variables
	Stage window;
	Scene scene;
	
   Label lb1;
   Label lb2;
   
	Button b1;
	Button b2;
	Button b3;
   
   ListView<String> listView;
   
   //main method to launch
	public static void main(String[] args) {
		launch(args);	
	}

	@Override
	public void start(Stage primaryStage) throws Exception{
		window = primaryStage;
		window.setTitle("Linked List");
	
		//I used grid the get the layout I wanted
		GridPane grid = new GridPane();
		//The padding is to essentially set the margins for which my nodes won't go past
		grid.setPadding(new Insets(10,10,10,10));
		grid.setVgap(10);
		grid.setHgap(15);
		
      //I set default values for the linked list so that the user can already choose from the list and see what is in there
      String[] values = {"thanksgiving", "pie", "turkey"};
		List<String> list1 = new LinkedList<String>();
		for(String x: values){
			list1.add(x);
      }//end for loop
 
      //label for Value Input TextField
      Label lb1 = new Label("Value Input");
		GridPane.setConstraints(lb1, 0, 0);
		
      //value TextField
		TextField valueInput = new TextField();
		//I set a constraint as to how wide it should be because it was messing up the buttons look
		valueInput.setMaxWidth(200);
		GridPane.setConstraints(valueInput, 1, 0);
		
      //label for index input TextField
      Label lb2 = new Label("Index Input");
		GridPane.setConstraints(lb2, 0, 2);
      
		//index TextField
		TextField indexInput = new TextField();
		//I set a constraint as to how wide it should be because it was messing up the buttons look
		indexInput.setMaxWidth(200);
		GridPane.setConstraints(indexInput, 1, 2);
		
		//label for button1
		//Button 1 searches the program for the desired value
		Button b1 = new Button("Search");
		GridPane.setConstraints(b1, 1, 4);
		//This button will take the input from the value textfield and search the linked list for it
		b1.setOnAction(e -> {
		
			String output = "";
			String value = "";
			value = valueInput.getText();
	      
         //use boolean to check if its in the linked list
			boolean retval = list1.contains(value);
	      //value is in the linked list
			if(retval == true){
            //create a string that can be displayed to the user with the results
				output = "the value: " + value + " was found at index: " + list1.indexOf(value)+"\n";
			}//end if
         //if not in the list
			else{
            //create a string that can be displayed to the user with the results
				output = "The value doesn't exist in the Linked List";
			}//end else
         //open popup window and display the output for the user
         display("Search", output);								
		});//end button action
		
		//label for button1
		//Button 2 inserts a value at a given index
		Button b2 = new Button("Insert");
		GridPane.setConstraints(b2, 2, 4);
		//This button takes the value and index inputs from the text fields and inserts it into the list
		b2.setOnAction(e ->  {
			int index = 0;
			String value = "";
	      String output = "";
         
			index = Integer.parseInt(indexInput.getText()); 
			value = valueInput.getText();
	      
         //checking to see if the index is in range of the list
			if(index <= list1.size()){
            list1.add(index, value);
            //update the listview with the value added
            listView.getItems().add(value);
         }//end if
         //if index isn't in range of the list it creates this popup window
         else{
         //popup window displays this output
         output = "Sorry, that index is out of range of the Linked List";
         display("Error", output);
         }//end else
        	
		});//end button action
		
		//label for button3
		//Button 3 deletes an a value from the LinkedList by taking in an index
		Button b3 = new Button("Delete");
		GridPane.setConstraints(b3, 3, 4);
		//This button takes the index from the index input text field and deletes it from thelist
		b3.setOnAction(e -> {
			int index = 0;
			index = Integer.parseInt(indexInput.getText());
	      String output = "";
         //checking to see if the index is in range of the list
         if(index <= list1.size()){
			   list1.remove(index);	
            //update the listview with the value removed
            listView.getItems().remove(index);
         }//end if
         //if not in range, popup window shows with error message
         else{
            //error message to be shown to the user in popup window
            output = "Sorry, that index range doesn't exist in the Linked List";
            display("Error", output);
         }//end else
		});//end button action
      	
      //listview to show the current list in the GUI
      listView = new ListView<>();
      listView.getItems().addAll(list1);
      listView.setMaxWidth(150);
      GridPane.setConstraints(listView, 1, 5);
      
      grid.getChildren().addAll(valueInput, indexInput, b1, b2, b3, lb1, lb2, listView);

      //set scene parameter to grid as the layout profile 
      scene = new Scene(grid, 550, 300);
      //set scene
      window.setScene(scene);
      //show display the stage
      window.show();
   }
   //create a method that makes a generic popup window that takes a title and input text
   public static void display(String title, String text){
		Stage window = new Stage();
		
		window.setTitle(title);
		window.setWidth(300);
		
		TextArea Out = new TextArea();
		//I set a constraint as to how wide it should be because it was messing up the buttons look
		Out.setMaxWidth(250);
		Out.setText(text);
		Button closeButton = new Button("Exit Window");
		closeButton.setOnAction(e -> window.close());
		
		VBox layout = new VBox(10);
		layout.getChildren().addAll(Out, closeButton);
		//center position everything
      layout.setAlignment(Pos.CENTER);
		
		Scene scene = new Scene(layout);
		window.setScene(scene);
		window.show();		
   }
}
    
		
		
		
		
		
		
		
		
		
		