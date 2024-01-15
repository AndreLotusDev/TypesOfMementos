# Memento Pattern in C#

## Overview

The Memento pattern is a behavioral design pattern that allows an object to save its state so that it can restore to this state later. This pattern is particularly useful for implementing features such as undo mechanisms in software.

## Intent

The intent of the Memento pattern is to capture and externalize an object's internal state so that the object can be restored to this state later, without violating encapsulation.

## Implementation

In C#, the Memento pattern can be implemented using three actor classes:

1. **Originator**: The class of objects whose state needs to be saved and restored.
2. **Memento**: The class that stores the internal state of the Originator object. The Memento should only be accessible by the Originator.
3. **Caretaker**: The class that keeps track of the Mementos but does not modify or examine the contents of the Memento.

## Example in C#

Here's a basic implementation example in C#:

```csharp
using System;

// Memento class
public class Memento
{
    public string State { get; private set; }

    public Memento(string state)
    {
        this.State = state;
    }
}

// Originator class
public class Originator
{
    public string State { get; set; }

    public Memento SaveStateToMemento()
    {
        return new Memento(State);
    }

    public void GetStateFromMemento(Memento memento)
    {
        State = memento.State;
    }
}

// Caretaker class
public class Caretaker
{
    private List<Memento> mementoList = new List<Memento>();

    public void Add(Memento state)
    {
        mementoList.Add(state);
    }

    public Memento Get(int index)
    {
        return mementoList[index];
    }
}

class Program
{
    static void Main(string[] args)
    {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.State = "State #1";
        originator.State = "State #2";
        caretaker.Add(originator.SaveStateToMemento());

        originator.State = "State #3";
        caretaker.Add(originator.SaveStateToMemento());

        originator.State = "State #4";
        Console.WriteLine("Current State: " + originator.State);        

        originator.GetStateFromMemento(caretaker.Get(0));
        Console.WriteLine("First saved State: " + originator.State);
        originator.GetStateFromMemento(caretaker.Get(1));
        Console.WriteLine("Second saved State: " + originator.State);
    }
}
```
