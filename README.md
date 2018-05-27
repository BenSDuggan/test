# ETag code revisions
ETag and code originally developed by [Eli Bridge](https://github.com/Eli-S-Bridge) and [Jay Wilhelm](https://github.com/jaywilhelm).
Revisions by [Ben Duggan](https://github.com/BenSDuggan).  You can email with questions at [bendugga@iu.edu](mailto:bendugga@iu.edu).

## **__Status: Testing code**__

* [Original cdoe with instillation help](https://github.com/Eli-S-Bridge/ETAG_4095_Apr2018)

## List of changes:
* Sleep function: To save battery life the board can go to sleep.  In the constants and loging parameters section slpH, slpM and slpS are hour, minute and second to put the board to sleep, respectively.  wakH, wakM and wakS are the hour, minute and second to wake up, respectively.  Both of these are in 24 hour format.  Some notes on this feature:
    * This feature only works for sleeping at night and waking up in the morning.  By this I mean that the following must hold: 
    ```
    0 <= wakH:wakM:wakS <= slpH:slpM:slpS <= 23:59:59
    ```
    * When the board goes to sleep the communication through the USB is terminated.  So if the board is scheduled to sleep and you try to upload code it won't work.  To fix this the board will terminate scanning and write to the log file when the board is scheduled to sleep, but the board won't go into its deep sleep for 2 minutes after a sleep is scheduled.
* Reader ID: In the constants and loging parameters section you can set readerID which is prepended to the data and log file.  This is similar to file name scheme of the Gen. 2 boards.  The string can contain alphanumeric characters.
* Log file: There is a log file added to the board.  This is similar to the one from the Gen. 2.  The file saves when logging is started and stopped.
* Create data and log file at intial startup:  The data and log files are created imediatly when the board is turned on, even if nothing is in them.
* Change file name to readerID + data/log: The file name scheme changed from just one that says datalog.txt to (readerID + "data.txt") for the data file and (readerID + "log.txt") for the log file.
