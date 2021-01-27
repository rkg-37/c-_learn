// https://www.cplusplus.com/forum/general/88109/

#include <iostream>
#include <memory>
using namespace std;

int main(){
	//this just builds a 2D array pi that looks like
	//0 1 2 3 4
	//5 6 7 8 9
	//10 11 12 13
	//...	   24
	int** pi=new int*[5];
	
	//this counter is augmented by 5 in every loop,
	//for the value to be 0...,5,..10etc
	int counter=0;
	for(int j=0;j<5;j++){	
		int* i=new int[5];
		for(int j=0;j<5;j++)
			i[j]=j+counter;
		counter=counter+5;

		pi[j]=i;
		//just to print out the array
		for(int k=0;k<5;k++)
			cout<<pi[j][k]<<" ";
		cout<<endl;
	}
	cout<<endl;

	//trying the same thing using smart pointers
	unique_ptr<int*[]> smp_pi(new int*[5]);
	counter=0;	
	for(int j=0;j<5;j++){
		unique_ptr<int[]> smp_i(new int[5]);		
		for(int k=0;k<5;k++){
			smp_i[k]=counter+k;		
			cout<<smp_i[k]<<" ";
		}
		counter=counter+5;
		cout<<endl;
		smp_pi[j]=&smp_i[0];
		//smp_pi[j]=smp_i; //this does not compile. why?
	}
	cout<<endl;

	for(int j=0;j<5;j++){	
		for(int k=0;k<5;k++)
			cout<<smp_pi[j][k]<<" ";
		cout<<endl;
	}

	return 0;
}



// changes to be implimented are 

  // *** change 1 ***
    unique_ptr< unique_ptr<int[]> [] > smp_pi(
        new unique_ptr<int[]>[5]
    );

    counter=0;
    for(int j=0; j<5; j++){
        unique_ptr<int[]> smp_i(new int[5]);
        for(int k=0; k<5; k++){
            smp_i[k] = counter+k;
            cout << smp_i[k] << " ";
        }
        counter=counter+5;
        cout<<endl;
        smp_pi[j] = move(smp_i);  // **** change 2
    }
    cout<<endl;

    for(int j=0;j<5;j++){
        for(int k=0;k<5;k++)
            cout<<smp_pi[j][k]<<" ";
        cout<<endl;
    }
