## Script to check if a user exists.
### In this script, we write code in bash to determiine if a user exists and if present in the wheel group. If not, we create the user and add to wheel group.


<pre><code> 
#!/bin/bash


# Author: Angela H
# Date: 5/28/2025
# Description: This code is to check if a user exists
#
#

user="Janet"
group="wheel"
#Check if user exists

if id "$user"; then
        echo "User $user exists."
else
        echo "User $user does not exist."
fi

# Check if user belongs to wheel group

if groups "$user" | grep "$group"; then
        echo "User $user is already in the $group group."
else
        echo "User $user is not in the $group group."

        # Add user to the wheel group
        sudo usermod -aG "$group" "$user"
fi

# Verify if user has been added to the wheel group

if id -nG "$user" | grep -q "$group"; then
        echo "User $user has been successfully added to $group group."
else
        echo "Failed to add $user to $group group."
        echo "User $user does not exist. Create user and add to $group group."
fi

# Since User does not exist; create the user and add them to the group

sudo useradd -m -G "$group" "$user"

# Verify user has been created

if getent passwd "$user"; then
        echo "User $user has been created."
else
        echo "Failed to create User $user."
fi

# Verify if user belong to wheel group

if id -nG "$user" | grep -q "$group"; then
        echo "User $user is in the $group group."
else
        echo "Failed to add $user to the $group group."
fi
</code></pre>
