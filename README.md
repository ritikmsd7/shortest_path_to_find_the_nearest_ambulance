# shortest_path_to_find_the_nearest_ambulance



#include <iostream>
#include<ctime>
#include<cstdlib>
#include<vector>
#include<algorithm>
using namespace std;

void total_time(vector<int> &time_taken,int &i,int &j,vector<vector<int>> &Traffic_lights){
    
    if(Traffic_lights[i][j]==0){
        time_taken.push_back(2);
    }
    else if(Traffic_lights[i][j]==1){
        time_taken.push_back(5);
    }
    else{
        time_taken.push_back(8);
    }
}
int minimum(int &sum1,int &sum2,int &sum3){

    
    
     if((sum1<0 || sum2<0 || sum3<0)){
        if(sum1<0){
            sum1=100;
        }
        if(sum2<0){
            sum2=100;
        }
        if(sum3<0){
            sum3=100;
        }
    }
    
    if(sum1<=sum2 && sum1<=sum3 ){
        return sum1;
    }

    else if(sum2<=sum3 ){
        return sum2;
    }

    else{
        return sum3;
    }


}

void smalldistance(int &i,int &j,vector<pair<int,int>> &output,int &sum1,int &sum2,int &sum3,vector<int> &distance,vector<vector<int>> &matrix,vector<vector<int>> &Traffic_lights,vector<int> &time_taken){

    if(minimum(sum1,sum2,sum3)==sum1){
        distance.push_back(matrix[i+1][j]);
        i=i+1;
        total_time(time_taken,i,j,Traffic_lights);
        output.push_back(make_pair(i,j));
    }

    else if(minimum(sum1,sum2,sum3)==sum2){
       distance.push_back(matrix[i+1][j+1]);
        i=i+1;
        j=j+1;
        //output.push_back('A');
        total_time(time_taken,i,j,Traffic_lights);
        output.push_back(make_pair(i,j));
    }

    else if(minimum(sum1,sum2,sum3)==sum3){
        distance.push_back(matrix[i][j+1]);
        j=j+1;
        //output.push_back('R');
        total_time(time_taken,i,j,Traffic_lights);
        output.push_back(make_pair(i,j));
    }

}
void shortestpathwithTrafficlights(int &row,int &col,int &i,int &j,vector<vector<int>> &matrix,vector<vector<int>> &Traffic_lights,vector<pair<int,int>> &output,vector<int>&distance,vector<int> &time_taken){



    if(i==row-1 && j==col-1){
       // cout<<output;
       cout<<"ok";
        return;
    }

   // cout<<output;
    while(i<row && i>=0 && j<col && j>=0){
       // cout<<i<<" "<<j<<endl;

    if(i==row-1 || j==col-1){
        if(i==row-1){
           distance.push_back(matrix[i][j+1]);
            j=j+1;
            //output.push_back('R');
            total_time(time_taken,i,j,Traffic_lights);
            output.push_back(make_pair(i,j));
        }

        if(j==col-1){
           distance.push_back(matrix[i+1][j]);
            i=i+1;
           // output.push_back('D');
           total_time(time_taken,i,j,Traffic_lights);
           output.push_back(make_pair(i,j));
        }
    }
    else if(Traffic_lights[i+1][j]==0 || Traffic_lights[i+1][j+1]==0 || Traffic_lights[i][j+1]==0 ){

        int sum1=-1,sum2=-1,sum3=-1;
        int count=0;
        if(i+1<row  && Traffic_lights[i+1][j]==0){
            sum1+=matrix[i+1][j];
            count++;
        }

        if(j+1<col  && Traffic_lights[i][j+1]==0){
            sum3+=matrix[i][j+1];
            count++;
            
        }

         if((i+1<row && j+1<col)  && Traffic_lights[i+1][j+1]==0){
            sum2+=matrix[i+1][j+1];
            count++;
        }

        if(count>0){
            smalldistance(i,j,output,sum1,sum2,sum3,distance,matrix,Traffic_lights,time_taken);
        }
        
        if(sum1==-2 && sum2==-2 && sum3==-2){
            cout<<"No path exist"<<endl;
            break;
        }
       if(sum1==-2 && sum2==-1 && sum3==-1){
           
             
            if(matrix[i][j+1]<0 || matrix[i+1][j+1]<0){
                if(matrix[i][j+1]<0){
                   // output.push_back('A');
                   distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                   // output.push_back('R');
                    distance.push_back(matrix[i][j+1]);
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i][j+1],matrix[i+1][j+1]);
           
           if(mini==matrix[i][j+1]){
              // output.push_back('R');
               distance.push_back(matrix[i][j+1]);
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
               //output.push_back('A');
                distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-2 && sum3==-1){
           
           if(matrix[i+1][j]<0 || matrix[i][j+1]<0){
                    if(matrix[i+1][j]<0){
                       // output.push_back('R');
                        distance.push_back(matrix[i][j+1]);
                        j=j+1;
                         total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
                    
                    else{
                        //output.push_back('D');
                        distance.push_back(matrix[i+1][j]);
                        i=i+1;
                         total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
              }
              
           int mini=min(matrix[i+1][j],matrix[i][j+1]);
           
           if(mini==matrix[i+1][j]){
              // output.push_back('D');
               distance.push_back(matrix[i+1][j]);
               i=i+1;
                total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('R');
               distance.push_back(matrix[i][j+1]);
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-1 && sum3==-2){
           
            if(matrix[i+1][j+1]<0 || matrix[i+1][j]){
                if(matrix[i+1][j+1]<0){
                   // output.push_back('A');
                    distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                  //  output.push_back('D');
                   distance.push_back(matrix[i+1][j]);
                    i=i+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i+1][j],matrix[i+1][j+1]);
           
           if(mini==matrix[i+1][j]){
                distance.push_back(matrix[i+1][j]);
             //  output.push_back('D');
               i=i+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
               //output.push_back('A');
              distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
    }

    else if(Traffic_lights[i+1][j]==1 || Traffic_lights[i+1][j+1]==1 || Traffic_lights[i][j+1]==1 ){
         int sum1=-1,sum2=-1,sum3=-1;
         int count=0; 
        if(i+1<row  && Traffic_lights[i+1][j]==1){
            sum1+=matrix[i+1][j];
            count++;
        }

        if(j+1<col && Traffic_lights[i][j+1]==1){
            sum3+=matrix[i][j+1];
            count++;
        }

        if((i+1<row && j+1<col)  && Traffic_lights[i+1][j+1]==1){
            sum2+=matrix[i+1][j+1];
            count++;
        }


        if(count>0){
            smalldistance(i,j,output,sum1,sum2,sum3,distance,matrix,Traffic_lights,time_taken);
        }
        
        if(sum1==-2 && sum2==-2 && sum3==-2){
            cout<<"No path exist"<<endl;
            break;
        }
        
         if(sum1==-2 && sum2==-1 && sum3==-1){
             
            if(matrix[i][j+1]<0 || matrix[i+1][j+1]<0){
                if(matrix[i][j+1]<0){
                   // output.push_back('A');
                    distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                    //output.push_back('R');
                    distance.push_back(matrix[i][j+1]);
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i][j+1],matrix[i+1][j+1]);
           
           if(mini==matrix[i][j+1]){
               //output.push_back('R');
                distance.push_back(matrix[i][j+1]);
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('A');
               distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-2 && sum3==-1){
        
            if(matrix[i+1][j]<0 || matrix[i][j+1]<0){
                    if(matrix[i+1][j]<0){
                       // output.push_back('R');
                        distance.push_back(matrix[i][j+1]);
                        j=j+1;
                        total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
                    
                    else{
                        //output.push_back('D');
                        distance.push_back(matrix[i+1][j]);
                        i=i+1;
                        total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
              }
           
           int mini=min(matrix[i+1][j],matrix[i][j+1]);
           
           if(mini==matrix[i+1][j]){
               //output.push_back('D');
                distance.push_back(matrix[i+1][j]);
               i=i+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
               //output.push_back('R');
                distance.push_back(matrix[i][j+1]);
               j=j+1;
                total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-1 && sum3==-2){
           
           if(matrix[i+1][j+1]<0 || matrix[i+1][j]){
                if(matrix[i+1][j]<0){
                    //output.push_back('A');
                    distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                    //output.push_back('D');
                    distance.push_back(matrix[i+1][j]);
                    i=i+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i+1][j],matrix[i+1][j+1]);
           
           if(mini==matrix[i+1][j]){
              // output.push_back('D');
               distance.push_back(matrix[i+1][j]);
               i=i+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('A');
               distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
    }

    else{
        int sum1=-1,sum2=-1,sum3=-1;
        int count=0; 
        if(i+1<row && Traffic_lights[i+1][j]==2){
            sum1+=matrix[i+1][j];
            count++;
        }

        if(j+1<col && Traffic_lights[i][j+1]==2){
            sum3+=matrix[i][j]+1;
            count++;
        }

        if((i+1<row && j+1<col) && Traffic_lights[i+1][j+1]==2){
            sum2+=matrix[i+1][j+1];
            count++;
        }

        if(count>0){
            smalldistance(i,j,output,sum1,sum2,sum3,distance,matrix,Traffic_lights,time_taken);
        }
        if(sum1==-2 && sum2==-2 && sum3==-2){
            cout<<"No path exist"<<endl;
            break;
        }
        
         if(sum1==-2 && sum2==-1 && sum3==-1){
             
               
            if(matrix[i][j+1]<0 || matrix[i+1][j+1]<0){
                if(matrix[i][j+1]<0){
                    //output.push_back('A');
                    distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                   // output.push_back('R');
                    distance.push_back(matrix[i][j+1]);
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i][j+1],matrix[i+1][j+1]);
           
           if(mini==matrix[i][j+1]){
              // output.push_back('R');
               distance.push_back(matrix[i][j+1]);
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('A');
               distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-2 && sum3==-1){
           
           if(matrix[i+1][j]<0 || matrix[i][j+1]<0){
                    if(matrix[i+1][j]<0){
                        //output.push_back('R');
                        distance.push_back(matrix[i][j+1]);
                        j=j+1;
                        total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
                    
                    else{
                       // output.push_back('D');
                        distance.push_back(matrix[i+1][j]);
                        i=i+1;
                        total_time(time_taken,i,j,Traffic_lights);
                        output.push_back(make_pair(i,j));
                    }
              }
              
           int mini=min(matrix[i+1][j],matrix[i][j+1]);
           
           if(mini==matrix[i+1][j]){
               //output.push_back('D');
               distance.push_back(matrix[i+1][j]);
               i=i+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('R');
               distance.push_back(matrix[i][j+1]);
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
       else if(sum1==-1 && sum2==-1 && sum3==-2){
           
            if(matrix[i+1][j+1]<0 || matrix[i+1][j]){
                if(matrix[i+1][j]<0){
                   // output.push_back('A');
                    distance.push_back(matrix[i+1][j+1]);
                    i=i+1;
                    j=j+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
                
                else{
                   // output.push_back('D');
                    distance.push_back(matrix[i+1][j]);
                    i=i+1;
                    total_time(time_taken,i,j,Traffic_lights);
                    output.push_back(make_pair(i,j));
                }
            }
            
           int mini=min(matrix[i+1][j],matrix[i+1][j+1]);
           
           if(mini==matrix[i+1][j]){
               //output.push_back('D');
                distance.push_back(matrix[i+1][j]);
               i=i+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
        
           else{
              // output.push_back('A');
               distance.push_back(matrix[i+1][j+1]);
               i=i+1;
               j=j+1;
               total_time(time_taken,i,j,Traffic_lights);
               output.push_back(make_pair(i,j));
           }
       }
       
    }
    }

}

void nearesthospital(vector<vector<int>>& Ambulancematrix,vector<vector<int>> &matrix,int &target,int &targetrow,int &targetcol){
    
    vector<int> v;
    for(int i=0;i<Ambulancematrix.size();i++){
         int count=0;
        for(int j=0;j<=i;j++){
            if(Ambulancematrix[i][j]==target){
                v.push_back(i);
                v.push_back(j);
            }
            count++;
        }
        
        for(int j=count-2;j>=0;j--){
            if(Ambulancematrix[j][i]==target){
                v.push_back(j);
                v.push_back(i);
            }
        }
    }
   
   int row1=v[0];
   int col1=v[1];
   
   vector<int> v2;
    
     for(int i=0;i<Ambulancematrix.size();i++){
         int count=0;
        for(int j=0;j<=i;j++){
            if(Ambulancematrix[j][i]==target){
                v2.push_back(j);
                v2.push_back(i);
            }
            count++;
        }
        
        for(int j=count-2;j>=0;j--){
            if(Ambulancematrix[i][j]==target){
                v2.push_back(i);
                v2.push_back(j);
            }
        }
    }
    
    
    
    int row2=v2[0];
    int col2=v2[1];
    
    if(row1==col2 && row2==col1){
       
       int count1,count2=0;
       
       for(int i=0;i<row1;i++){
           if(matrix[0][i]==-1){
               count1++;
           }
       }
       
       for(int i=0;i<col2;i++){
           if(matrix[i][0]==-1){
               count2++;
           }
       }
       
       if(count1>=count2){
           targetrow=row1;
           targetcol=col1;
       }
       else{
           targetrow=row2;
           targetcol=col2;
       }
    }
    else{
   
     if(row1<row2){
         targetrow=row1;
         targetcol=col1;
     }
     
     else if(row1==row2){
         
         if(col1<col2){
             targetrow=row1;
             targetcol=col1;
         }
         
         else if(col1==col2){
             targetrow=row1;
             targetcol=col1;
         }
         
         else{
             targetrow=row2;
             targetcol=col2;
         }
     }
     
     else{
         targetrow=row2;
         targetcol=col2;
     }
    }
    
    
}
int main()
{
    
    
    string disease;
    cout<<"Enter the disease: "<<endl;
    getline(cin,disease);
    int target=0;
    if(disease=="heart"){
        target=2;
    }
    
    else if(disease=="cold" || disease=="sneezing" || disease=="fever"){
        target=0;
    }
    
    else{
        target=1;
    }
    
    srand(time(0));
    int row1,col1;
    cout<<"Enter the number of row:"<<endl;
    cin>>row1;
    cout<<"Enter the number of col:"<<endl;
    cin>>col1;
    cout<<endl;
    vector<vector<int>> matrix(row1,vector<int>(col1,0));

   cout<<"Random matrix: "<<endl;
    for(int i=0;i<matrix.size();i++){
        for(int j=0;j<matrix[i].size();j++){

          if(i==j){
              matrix[i][j]=-1;
          }
          else if(rand()%3==2)
            matrix[i][j]=-1;
          else
            matrix[i][j]=(rand()%9)+1;
        }
    }

    // random matrix
     for(int i=0;i<matrix.size();i++){
        for(int j=0;j<matrix[i].size();j++){
            cout<<matrix[i][j]<<" ";
        }
        cout<<endl;
    }

    cout<<endl;
   vector<vector<int>> Traffic_lights(row1,vector<int>(col1,0));
   cout<<"Traffic lights matrix: "<<endl;
   for(int i=0;i<Traffic_lights.size();i++){
       for(int j=0;j<Traffic_lights[i].size();j++){
           Traffic_lights[i][j]=rand()%3;
       }
   }

   for(int i=0;i<Traffic_lights.size();i++){
       for(int j=0;j<Traffic_lights[i].size();j++){
           cout<<Traffic_lights[i][j]<<" ";
       }
       cout<<endl;
   }
   cout<<endl;
   vector<vector<int>> Ambulancematrix(row1,vector<int>(col1,0));
   cout<<"Ambulance Matrix: "<<endl;
   for(int i=0;i<Ambulancematrix.size();i++){
       for(int j=0;j<Ambulancematrix[i].size();j++){
           Ambulancematrix[i][j]=rand()%3;
       }
   }

   for(int i=0;i<Ambulancematrix.size();i++){
       for(int j=0;j<Ambulancematrix.size();j++){
           cout<<Ambulancematrix[i][j]<<" ";
       }
       cout<<endl;
   }
  int row=0;
  int col=0;
  nearesthospital(Ambulancematrix,matrix,target,row,col);
  cout<<row<<" "<<col<<endl;
  
  row+=1;
  col+=1;
   
  if(row==1 && col==1){
      cout<<endl;
      cout<<"You are already their"<<endl;
      cout<<"Total Distance: "<<"0"<<"kilometer"<<endl;
      cout<<"Total Time: "<<"0"<<"second"<<endl;
  }
  else if(matrix[row-1][col-1]==-1){
      cout<<endl;
      cout<<"Not a valid place";
  }
  else{
  //string output="";
  vector<pair<int,int>> output;
  vector<int> distance;
  vector<int> time_taken;
  int i=0;
  int j=0;
  shortestpathwithTrafficlights(row,col,i,j,matrix,Traffic_lights,output,distance,time_taken);
  //output.pop_back();
  cout<<endl;
  cout<<"The shortest path is: "<<endl;
  cout<<"(0,0)->";
  
  for(i=0;i<output.size()-1;i++){
      if(i!=output.size()-2){
          cout<<"("<<output[i].first<<","<<output[i].second<<")"<<"->";
      }
      else{
          cout<<"("<<output[i].first<<","<<output[i].second<<")";
      }
  }
  cout<<endl;
  int total_distance=0;
  for(int i=0;i<distance.size()-1;i++){
      if(distance[i]==-1){
          total_distance+=0;
      }
      else{
          total_distance+=distance[i];
      }
  }
  cout<<endl;
  cout<<"Total Distance: "<<total_distance<<"kilometer";
  cout<<endl;
  int shortesttime=0;
  for(int i=0;i<time_taken.size()-1;i++){
      shortesttime+=time_taken[i];
  }
  cout<<endl;
  cout<<"Time Taken: "<<shortesttime<<"seconds";
  }
  


}


