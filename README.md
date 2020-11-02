# The Attacker IP Prioritizer V2.0.3

## The Idea

The Attacker IP Prioritizer (AIP) algorithm aims to generate a IoT friendly blacklist. With the advent of 5G, many IoT devices are going to be directly connected to the internet instead of being protected by a routers firewall. Therefore we need blacklists that are small and portable, and designed to blacklist IPs that are targeting IoT. Blacklists are not the be all end all of security since they are past-data bound, and as such they can never fully protect a device, and a full IPS solution is usually needed. However, if Blacklists contain the IPs that are the most aggressive and dangerous, most attacks can be stopped, thus lowering the stress on the full IPS solution. AIP aims at providing a data and statistics driven blacklist that only contains the IPs that NEED to be blocked.

## Data Source

The format for the input data is a directory that contains data files from each day. You assign a directory for the program to look in every time it runs, and it checks if there are any new files to process. If there are, it processes the new files and remembers the names of the new files so that it does not process it the next time it runs.

In terms of file format, it accepts a .csv file that has one IP per line, with each of the following data inputs for each IP on that line, separated by commas:

    Amount of events - Meaning the total connections to our honeypots originating from the given IP

    Total Duration - How long did this IP connect for the total of its events

    Average duration - The average length in seconds of all the connections per IP

    Amount of Bytes - Total bytes sent and received

    Average number of bytes - For bytes transferred in each connection per IP

    Total packets - Of all the connections per IP

    Average packets - Average packets sent per connection

    Last event time - UNIX time of the last time the IP tried to connect to something  in the last 24 hours

    First event time - UNIX time of the first time the IP tried to connect in the last 24 hours

For example, a single line in the file could look like this:

"IPv4 Address",26049,"7415310","284.6","41808957","1605.0",284577,"10.92","157899154","1578968762.519"
