# Error-handling-in-shell-scripting
What’s Error Handling in Shell Scripts All About?

When you write a shell script, you’re basically giving the computer a set of instructions to follow. But what happens if something goes wrong — like a file isn’t found, or a command fails?

**Error handling** is how we deal with those “uh-oh” moments. It helps the script recognize when something didn’t go as planned and decide what to do next — maybe show a friendly error, stop running, or clean up before exiting.

---

## 🧠 Why Check the Exit Status?

Every command you run in the shell quietly tells you whether it worked:

* If it **succeeded**, it returns `0`
* If it **failed**, it returns a **non-zero number**

That number is stored in a special variable called `$?`. You can check it to figure out if things went smoothly or not.


# Function to create S3 buckets for different departments
create_s3_buckets() {"\n    company=\"datawise\"\n    departments=(\"Marketing\" \"Sales\" \"HR\" \"Operations\" \"Media\")\n    \n    for department in \"${departments[@]"}"; do
        bucket_name="${company}-${department}-Data-Bucket"
        
        # Check if the bucket already exists
        if aws s3api head-bucket --bucket "$bucket_name" &>/dev/null; then
            echo "S3 bucket '$bucket_name' already exists."
        else
            # Create S3 bucket using AWS CLI
            aws s3api create-bucket --bucket "$bucket_name" --region your-region
            if [ $? -eq 0 ]; then
                echo "S3 bucket '$bucket_name' created successfully."
            else
                echo "Failed to create S3 bucket '$bucket_name'."
            fi
        fi
    done
}


## 🔍 What’s Happening Here?

* The script tries to create an s3 bucket.
* It immediately checks if the s3 buckets exist before creating any one.
* The error handling strategy  (check exit status) is executed to see if it worked using `$?`.
* If it didn’t, it prints an error and stops the script with `Failed to create S3 bucket '$bucket_name'`.
* If it did work, it lets the user know everything’s good.

---

## 🧭 When Should You Do This?

You’ll want to check for errors **anytime you run something important** that might fail:

* Moving or deleting files
* Downloading something from the internet
* Installing software
* Connecting to remote servers

Basically: **if something goes wrong, you don’t want your script to just keep going blindly** — that’s how bad things happen.

---

## 👋 Final Thought

Checking `$?` is a small thing that makes your scripts way more reliable and user-friendly. It’s like adding seatbelts to your automation — you hope you don’t need them, but you’ll be really glad they’re there if something fails.


For large scripts, combine this approach with other techniques like:

* `set -e`: To stop on any error
* `trap`: To perform cleanup
* Logging output for review
  
Allow me present an example
  ## ✅ A Simple Example

Here’s a small bash script that tries to copy a file and handles any issues if the copy doesn’t work:
#!/bin/bash

echo "Trying to copy the file..."

cp myfile.txt /backup/

if [ $? -ne 0 ]; then
    echo "❌ Oops! Something went wrong while copying."
    exit 1
else
    echo "✅ File copied successfully!"
fi


