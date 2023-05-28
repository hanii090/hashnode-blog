---
title: "Boto3 and Python: Simple DynamoDB CRUD Operations Examples"
seoDescription: "Master DynamoDB CRUD operations using Python and boto3, including creating, reading, updating, and deleting data in a simple database. Learn efficient..."
datePublished: Sun Apr 16 2023 21:29:10 GMT+0000 (Coordinated Universal Time)
cuid: clgjx6hqh00000ajzeh67eq6g
slug: boto3-and-python-simple-dynamodb-crud-operations-examples
tags: aws, python, dynamodb, python3, python-beginner

---

I recently completed a project where I had to interact with DynamoDB via boto3 (AWS SDK for Python). In this article, we'll create a simple DynamoDB database and go over how to create, read, update, and delete using Python and boto3.

---

If you didn’t know, DynamoDB is a fully managed, NoSQL database found in AWS Cloud environments. But I assume you know all about it if you’re reading this article, so I’ll move on.

## **Environment Setup**

Let’s create a new directory, cd into it, and create a **main.py** file. We’ll also create a virtual environment and activate it:

```bash
mkdir aws-dynamodb-test
cd aws-dynaomdb-test
touch main.py
python3 -m venv venv
source venv/bin/activate
```

Next, we’ll pip install boto3 which is the AWS Python SDK.

```bash
pip install boto3
```

In your **main.py** file, go ahead and import boto3 and set the tableName variable to your dynamodb table name. In our case, it will be `users`

```python
import boto3
tableName = 'users'
```

And create the dynamodb resource:

```python
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
db = dynamodb.Table(tableName)
```

Next, be sure you’ve authenticated with AWS via the CLI. If you don’t have it, download it and run `aws configure` and enter your account’s programmatic key and secret.

## **Create Table**

In our table, we’ll make the user’s last name the primary key and their age the sort key (range). Having both of these allow us to have primary keys of the same name but queryable by age. Without a range, primary keys must be unique.

```python
table = dynamodb.create_table(
    TableName=tableName,
    KeySchema=[
        {
            'AttributeName': 'last_name',
            'KeyType': 'HASH' 
        },
        {
            'AttributeName': 'age',
            'KeyType': 'RANGE' 
        },
    ],
    AttributeDefinitions=[
        {
            'AttributeName': 'last_name',
            'AttributeType': 'S'
        },
        {
            'AttributeName': 'age',
            'AttributeType': 'N' 
        },
    ],
    ProvisionedThroughput={
        'ReadCapacityUnits': 5,
        'WriteCapacityUnits': 5
    }
)
```

## **Put Item**

Now let’s add three users to our table:

```python
item1 = db.put_item(
    Item={
        'last_name': 'Johnson',
        'first_name': 'Benjamin',
        'age': 28,
        'id': 49387266
        }
    )
print(item1)

item2 = db.put_item(
    Item={
        'last_name': 'Jones',
        'first_name': 'Mary',
        'age': 42,
        'id': 49387267
        }
    )
print(item2)

item3 = db.put_item(
    Item={
        'last_name': 'Johnson',
        'first_name': 'Joe',
        'age': 33,
        'id': 49387268
        }
    )
print(item3)
```

## **Create Item**

The Put command above actually **creates** a new item if it doesn’t exist and **updates** it if it does. So we’ll move on.

## **Get Item**

Since we have a primary key AND range we have to specify both to get an item.

```python
user = db.get_item(
    Key={
        'last_name': 'Johnson',
        'age': 28
    }
)
print(user['Item'])
```

## **Get All Items (Scan)**

To get all items we use:

```python
allUsers = db.scan()
print(allUsers)
```

## **Query Items**

You can use the scan method to query items like so:

```python
#Get all users with last name of Johnson
johnsons = db.scan(
    FilterExpression=Key('last_name').eq("Johnson")
)
print(johnsons)

#Get all users over the age of 30
overThirty = db.scan(
    FilterExpression=Key('age').gt(30)
)
print(overThirty['Items'])
```

But scan operations are less efficient. It always scans the entire table AND then filters out values that you want (extra step).

Query cuts that last step out.

Using the query method **REQUIRES** the primary key to be passed.

In this example, we want all users with the last name of Johnson that fall in the age range of 30-40. Note the ProjectExpression allows us to choose what fields we return. In this case, we’ll return all.

```python
middleAges = db.query(
    ProjectionExpression="last_name, first_name, age, id",
    KeyConditionExpression=
        Key('last_name').eq('Johnson') & Key('age').between(30,40)
)
print(middleAges['Items'])
```

## **Delete Items**

And finally, to delete items:

```python
    goodbyeJohnson = db.delete_item(
        Key={
            'last_name': 'Johnson',
            'age': 33
        }
    )
```

## **Conclusion**

I hope these DynamoDB CRUD examples were helpful. Let me know of any questions below.