# OS-Project-codes
#include <iostream>
#include <string>

using namespace std;

struct process_Struct
{
    string process_name;
    int arrival_time, burst_time, completion_time, remaining;
};

int faculty_Queue(int no_of_process, int quantum_time)
{
    process_Struct faculty_Process[no_of_process];
    process_Struct temp_Struct;
    
    for (int count = 0; count < no_of_process; count++)
    {
        cout << "Enter the details of Process[" << count + 1 << "]" << endl;
        cout << "Process Name: ";
        cin >> faculty_Process[count].process_name;

        cout << "Arrival Time: ";
        cin >> faculty_Process[count].arrival_time;

        cout << "Burst Time: ";
        cin >> faculty_Process[count].burst_time;
        cout << endl;
    }

    for (int count = 0; count < no_of_process; count++)
    {
        for (int x = count + 1; x < no_of_process; x++)
        {
            if (faculty_Process[count].arrival_time > faculty_Process[x].arrival_time)
            {
                temp_Struct = faculty_Process[count];
                faculty_Process[count] = faculty_Process[x];
                faculty_Process[x] = temp_Struct;
            }
        }
    }

    for (int count = 0; count < no_of_process; count++)
    {
        faculty_Process[count].remaining = faculty_Process[count].burst_time;
        faculty_Process[count].completion_time = 0;
    }

    int total_time = 0;
    int queue = 0;
    int round_robin[100];
    round_robin[queue] = 0;

    int flag = 1;
    int n = 0;
    int z;
    do
    {
        flag = 0;
        
        for (int count = 0; count < no_of_process; count++)
        {
        	//here it checks whether there is any process present int the round robin queue if z incrememtns by 1.
            if (total_time >= faculty_Process[count].arrival_time)
            {
                z = 0;
                for (int x = 0; x <= queue; x++)
                {
                    if (round_robin[x] == count)
                    {
                        z++;
                    }
                }
                //if z is 0 then it will add the process to the round robin queue.
                if (z == 0)
                {
                    queue++;
                    round_robin[queue] = count;
                }
            }
        }

        if (queue == 0)
        {
            n = 0;
        }
        else
        {
            if (faculty_Process[round_robin[n]].remaining == 0)
            {
                n++;
            }
            if (n > queue)
            {
                n = (n - 1) % queue;//remainder
            }
        }
        if (faculty_Process[round_robin[n]].remaining > 0)
        {
            if (faculty_Process[round_robin[n]].remaining < quantum_time)
            {
                total_time += faculty_Process[round_robin[n]].remaining;
                faculty_Process[round_robin[n]].remaining = 0;
            }
            else
            {
                total_time += quantum_time;
                faculty_Process[round_robin[n]].remaining -= quantum_time;
            }
            faculty_Process[round_robin[n]].completion_time = total_time;
        }
        n++;
        flag = 0;
        for (int count = 0; count < no_of_process; count++)
        {
            if (faculty_Process[count].remaining > 0)
            {
                flag = 1;
                break;
            }
        }
    } while (flag == 1);

    cout << "-------------------------------------" << endl;
    cout << "| \t Process \t | \t Completion Time \t|" << endl;
    cout << "-------------------------------------" << endl;
    for (int count = 0; count < no_of_process; count++)
    {
        cout << "| \t " << faculty_Process[count].process_name << "\t\t      | \t\t " << faculty_Process[count].completion_time << "\t\t  |" << endl;
        cout << "-------------------------------------" << endl;
    }

    int total_waiting_time = 0;
    int total_turnaround_time = 0;

    for (int count = 0; count < no_of_process; count++) //used to itterate over the all process
    {
        int waiting_time = faculty_Process[count].completion_time - faculty_Process[count].burst_time - faculty_Process[count].arrival_time;
        total_waiting_time += waiting_time;
        int turnaround_time = faculty_Process[count].completion_time - faculty_Process[count].arrival_time;
        total_turnaround_time += turnaround_time;
    }

    float average_waiting_time = static_cast<float>(total_waiting_time) / no_of_process;
    float average_turnaround_time = static_cast<float>(total_turnaround_time) / no_of_process;

    cout << "Average Waiting Time: " << average_waiting_time << endl;
    cout << "Average Turnaround Time: " << average_turnaround_time << endl;

    return 0;
}

int main()
{
    int no_of_process;
    int quantum_time;

    cout << "Enter the number of processes: ";
    cin >> no_of_process;

    cout << "Enter the quantum time: ";
    cin >> quantum_time;

    faculty_Queue(no_of_process, quantum_time);

    return 0;
}
