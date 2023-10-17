# ShellScript_Assg

### Write a shell script to change the values in a file(i.e sig.conf) according to the input passed to the script. The script should ask for all four inputs from the user & also validate the input.Below are the details of input. In full bracket options are given, you have to restrict the user pass single value for each input from the provided options in the full bracket.
Input:-
1) Component Name [INGESTOR/JOINER/WRANGLER/VALIDATOR]
2) Scale [MID/HIGH/LOW]
3) View [Auction/Bid]
4) Count [single digit number]
Explanation of a conf file line.
<view> ; <scale> ; <component name> ; ETL ; vdopia-etl= <count>
Note:- vdopiasample stands for Auction & vdopiasample-bid is for Bid
The script should change the values in the file according to the input provided. At a time only one line of the conf file should be altered.<br><br>
**Ans:-** <br>
 **Step 1:** Create a Shell Script.<br>
   +  Create a new file, **script.sh** in the Desktop , and add the following lines to it.<br>
``` ble.sh
#!/bin/bash

# Function to validate input against given options
validate_input() {
    local input="$1"
    local options="$2"
    for option in $options; do
        if [ "$input" == "$option" ]; then
            return 0  # Input is valid
        fi
    done
    return 1  # Input is invalid
}

# Prompt user for input values
read -p "Enter Component Name [INGESTOR/JOINER/WRANGLER/VALIDATOR]: " component
validate_input "$component" "INGESTOR JOINER WRANGLER VALIDATOR" || { echo "Invalid input!"; exit 1; }

read -p "Enter Scale [MID/HIGH/LOW]: " scale
validate_input "$scale" "MID HIGH LOW" || { echo "Invalid input!"; exit 1; }

read -p "Enter View [Auction/Bid]: " view
validate_input "$view" "Auction Bid" || { echo "Invalid input!"; exit 1; }

read -p "Enter Count [single digit number]: " count
if ! [[ "$count" =~ ^[0-9]$ ]]; then
    echo "Invalid input! Count must be a single digit number."
    exit 1
fi

# Determine the appropriate view string
if [ "$view" == "Auction" ]; then
    view_string="vdopiasample"
elif [ "$view" == "Bid" ]; then
    view_string="vdopiasample-bid"
else
    echo "Invalid input! View must be either 'Auction' or 'Bid'."
    exit 1
fi

# Modify the sig.conf file
echo  "/^$view_string ; $scale ; $component ; ETL ; vdopia-etl=$count/" > /home/sigmoid/sig.conf

echo "sig.conf file has been updated successfully."
```
**Step 2:** Create sig.conf File.<br>
 >      nano sig.conf
 + sig.conf file and Shell Script file should be in the same directory.<br> 
 **Step 3:** Make the Script Executable.<br>
 >      chmod 755 script.sh
**Step 4:** Run the Script.<br>
>       ./script.sh
**Step 5:**Verify the sig.conf File.<br>
+ After running the script, check the sig.conf file to confirm that the specified line has been updated with the new values according to the input.
