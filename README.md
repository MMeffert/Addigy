# Addigy
Scripts and code related to automating tasks in the Addigy MDM

## Smart Software Install for HP Laserjet 600 M602 Printer
1. Follow the directions from HP to get the drivers for your printer model.
https://support.hp.com/us-en/document/c06164609#one_download_EasyAdmin
2. Login to **Addigy**, click **Catalog** on the left side, click **Software**, Click the **New** button on the right side
3. Enter a name for the software in the **Software name** field. Example: Install Printer - BWPrinter
4. Enter a version number.  This is optional but I entered the version of the HP driver that was downloaded in step 1 which was 6.1.0.1 at the time of me writing these instructions.
5. Click the **Select File(s)** button, click the **Upload New** button and select the **.gz** file that you downloaded from HP Easy Admin in step 1. Example: hp-printer-essentials-UniPS-6_1_0_1.pkg
6. Click the **Upload and Mark as Selected** button
7. Make sure the checkbox is selected next to your uploaded file and click the **Select** button at the bottom of the page.
8. Click the **Add** link under Install Command.  This will give you the basic driver install command.
9. Add the following code above the automatically generated code from step 8.  This will make sure the drivers are not reinstalled if they already exist.

```bash
FILE="/Library/Printers/PPDs/Contents/Resources/HP LaserJet 600 M601 M602 M603.gz"

#Check if HP drivers are already installed and install if they are not
if [ ! -f $FILE ]
then
```
10. Add the following code below the automatically generated code from step 8.

```bash
fi

#Create the printer
lpadmin -p BWPrinter -L "Middle Office Printer Area" -E -v ipp://10.11.1.127 -P "/Library/Printers/PPDs/Contents/Resources/HP LaserJet 600 M601 M602 M603.gz"
```
11. Click the **Save** button at the bottom of the page.

### Complete Script ###
```bash
FILE="/Library/Printers/PPDs/Contents/Resources/HP LaserJet 600 M601 M602 M603.gz"

#Check if HP drivers are already installed and install if they are not
if [ ! -f $FILE ]
then
/usr/sbin/installer -pkg "/Library/Addigy/ansible/packages/Install Printer - BWPrinter (1.0)/hp-printer-essentials-UniPS-6_1_0_1.pkg" -target /
fi

#Create the printer
lpadmin -p BWPrinter -L "Middle Office Printer Area" -E -v ipp://10.11.1.127 -P "/Library/Printers/PPDs/Contents/Resources/HP LaserJet 600 M601 M602 M603.gz"
```

### Future Updates ###
- Condition for Install script
- Removal Command
