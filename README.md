# Assignment-1
/*Consider telephone book database of N clients. Make use of a  hash table implementation to quickly look up client‘s telephone  number. Make use of two collision handling techniques and  compare them using number of comparisons required to find a set  of telephone numbers*/


class hashTable:
    def __init__(self):  # Changed _init_ to __init__
        self.size = int(input("Enter the Size of the hash table : "))
        self.table = [None] * self.size
        self.elementCount = 0
        self.comparisons = 0
    def isFull(self):
        return self.elementCount == self.size
    def hashFunction(self, element):
        return element % self.size
    def insert(self, record):
        if self.isFull():
            print("Hash Table Full")
            return False
        position = self.hashFunction(record.get_number())
        if self.table[position] is None:
            self.table[position] = record
            print("Phone number of %s is at position %d" % (record.get_name(), position))
            self.elementCount += 1
        else:
            print("Collision has occurred for %s's phone number at position %d. Finding new Position." % (record.get_name(), position))
            while self.table[position] is not None:
                position = (position + 1) % self.size  # Circular probing
            self.table[position] = record
            print("Phone number of %s is at position %d" % (record.get_name(), position))
            self.elementCount += 1
        return True
    def search(self, initials):
        found = False
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_name().startswith(initials):
                print("Phone number found at position %d: %s" % (i, self.table[i]))
                found = True
        if not found:
            print("Record not found")
        return found
    def search_by_key(self, key):
        found = False
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_number() == key:
                print("Phone number found at position %d: %s" % (i, self.table[i]))
                found = True
        if not found:
            print("Record not found")
        return found
    def delete(self, name):
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_name() == name:
                self.table[i] = None
                self.elementCount -= 1
                print("Record for %s deleted successfully." % name)
                return True
        print("Record not found for deletion.")
        return False
    def display(self):
        print("\nHash Table:")
        for i in range(self.size):
            print("Hash Value: %d\t\t%s" % (i, self.table[i]))
        print("The number of phonebook records in the Table are : %d" % self.elementCount)
class doubleHashTable:
    def __init__(self):  # Changed _init_ to __init__
        self.size = int(input("Enter the Size of the hash table : "))
        self.table = [None] * self.size
        self.elementCount = 0
        self.comparisons = 0
    def isFull(self):
        return self.elementCount == self.size
    def h1(self, element):
        return element % self.size
    def h2(self, element):
        return 5 - (element % 5)
    def doubleHashing(self, record):
        posFound = False
        limit = self.size
        i = 1
        while i <= limit:
            newPosition = (self.h1(record.get_number()) + i * self.h2(record.get_number())) % self.size
            if self.table[newPosition] is None:
                posFound = True
                break
            else:
                i += 1
        return posFound, newPosition
    def insert(self, record):
        if self.isFull():
            print("Hash Table Full")
            return False
        posFound = False
        position = self.h1(record.get_number())
        if self.table[position] is None:
            self.table[position] = record
            print("Phone number of %s is at position %d" % (record.get_name(), position))
            self.elementCount += 1
        else:
            print("Collision has occurred for %s's phone number at position %d. Finding new Position." % (record.get_name(), position))
            while not posFound:
                posFound, position = self.doubleHashing(record)
                if posFound:
                    self.table[position] = record
                    print("Phone number of %s is at position %d" % (record.get_name(), position))
                    self.elementCount += 1
        return posFound
    def search(self, initials):
        found = False
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_name().startswith(initials):
                print("Phone number found at position %d: %s" % (i, self.table[i]))
                found = True
        if not found:
            print("Record not found")
        return found
    def search_by_key(self, key):
        found = False
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_number() == key:
                print("Phone number found at position %d: %s" % (i, self.table[i]))
                found = True
        if not found:
            print("Record not found")
        return found
    def delete(self, name):
        for i in range(self.size):
            if self.table[i] is not None and self.table[i].get_name() == name:
                self.table[i] = None
                self.elementCount -= 1
                print("Record for %s deleted successfully." % name)
                return True
        print("Record not found for deletion.")
        return False
    def display(self):
        print("\nHash Table:")
        for i in range(self.size):
            print("Hash Value: %d\t\t%s" % (i, self.table[i]))
        print("The number of phonebook records in the Table are : %d" % self.elementCount)


class Record:
    def __init__(self):  # Changed _init_ to __init__
        self._name = None
        self._number = None
    def get_name(self):
        return self._name
    def get_number(self):
        return self._number
    def set_name(self, name):
        self._name = name
    def set_number(self, number):
        self._number = number
    def __str__(self):  # Changed _str_ to __str__
        return "Name: %s \t Number: %d" % (self.get_name(), self.get_number())

def input_record():
    record = Record()
    name = input("Enter Name: ")
    number = int(input("Enter Number: "))
    record.set_name(name)
    record.set_number(number)
    return record


choice1 = 0
while choice1 != 3:
    print("*********************")
    print("*1. Linear Probing  *")
    print("*2. Double Hashing  *")
    print("*3. Exit            *")
    print("*********************")
    choice1 = int(input("Enter Choice: "))
    if choice1 > 3:
        print("Please Enter Valid Choice")
    if choice1 == 1:
        h1 = hashTable()
        choice2 = 0
        while choice2 != 6:
            print("***********************************")
            print("*1. Insert                        *")
            print("*2. Search by Initials            *")
            print("*3. Search by Key (Phone number)  *")
            print("*4. Delete                        *")
            print("*5. Display                       *")
            print("*6. Back                          *")
            print("***********************************")
            choice2 = int(input("Enter Choice: "))
            if choice2 > 6:
                print("Please Enter Valid Choice")
            if choice2 == 1:
                record = input_record()
                h1.insert(record)
            elif choice2 == 2:
                initials = input("Enter Name Initials to Search: ")
                h1.search(initials)
            elif choice2 == 3:
                key = int(input("Enter Phone number to Search: "))
                h1.search_by_key(key)
            elif choice2 == 4:
                name_to_delete = input("Enter Name to Delete: ")
                h1.delete(name_to_delete)
            elif choice2 == 5:
                h1.display()
    elif choice1 == 2:
        h2 = doubleHashTable()
        choice2 = 0
        while choice2 != 6:
            print("***********************************")
            print("*1. Insert                        *")
            print("*2. Search by Initials            *")
            print("*3. Search by Key (Phone number)  *")
            print("*4. Delete                        *")
            print("*5. Display                       *")
            print("*6. Back                          *")
            print("***********************************")
            choice2 = int(input("Enter Choice: "))
            if choice2 > 6:
                print("Please Enter Valid Choice")
            if choice2 == 1:
                record = input_record()
                h2.insert(record)
            elif choice2 == 2:
                initials = input("Enter Name Initials to Search: ")
                h2.search(initials)
            elif choice2 == 3:
                key = int(input("Enter Phone number to Search: "))
                h2.search_by_key(key)
            elif choice2 == 4:
                name_to_delete = input("Enter Name to Delete: ")
                h2.delete(name_to_delete)
            elif choice2 == 5:
                h2.display()
