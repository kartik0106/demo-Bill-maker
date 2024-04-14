# demo-Bill-maker
from tkinter import *
from tkinter import ttk
from tkinter import messagebox
import os , sys

import tempfile


class College:
    def __init__(self,window):
        self.win=window
        
        self.years=["2020-21","2021-22","2022-23","2023-24","2024-25",]
        self.sem=["1","2","3","4","5","6","7","8"]
        self.branch=["IT","CSE","MECH","CHE","ARCH","Civil","Electrical","Electronics"]
        
        #variables
        self.sname=StringVar()
        self.smob=StringVar()
        self.sprn=StringVar()
        self.billNo=StringVar()
        
        self.amount=IntVar()
        self.tlist=[]
        
        self.win.geometry("1550x800+0+0")
        space=" "
        self.win.title(space*200+"BILL RECEIPT")
        heading=Label(self.win,text="Welcome to MGM University",background="saddlebrown",fg="white",font=("plain",15))
        heading.pack(fill=X,ipady=10)


        main_frame=Frame(self.win,background="grey")
        main_frame.pack(fill="both",expand=1)

        student_frame=LabelFrame(main_frame,pady=20,height=100,background="tan",text="Student details",font=("times new roman",15))
        student_frame.place(x=0,y=0,width=1550)

        form_frame=LabelFrame(main_frame,height=450,pady=80,padx=60,width=585,background="light blue",text="FEE details",font=("times new roman",15))
        form_frame.place(x=0,y=100)

        table_frame=LabelFrame(main_frame,height=0,pady=40,padx=50,width=0,background="white",text="BILL details",font=("times new roman",15))
        table_frame.place(x=600,y=100)

        button_frame=LabelFrame(main_frame,height=168,width=585,background="skyblue",text="Click",font=("arial",15))
        button_frame.place(x=0,y=565)

        #Student details
        Student_Name_lbl=Label(student_frame,text="Enter Student name",font=("times new roman",15))
        Student_Name_lbl.place(x=10,y=0,width=200)
        Student_Name_txt=Entry(student_frame,font=("time new roman",15),textvariable=self.sname)
        Student_Name_txt.place(x=250,y=0)

        Student_MobNo_lbl=Label(student_frame,text="Enter mobile number",font=("times new roman",15))
        Student_MobNo_lbl.place(x=500,y=0,width=210)
        Student_MobNo_txt=Entry(student_frame,font=("time new roman",15),textvariable=self.smob)
        Student_MobNo_txt.place(x=750,y=0)

        Student_PRN_lbl=Label(student_frame,text="Enter Roll no",font=("times new roman",15))
        Student_PRN_lbl.place(x=1000,y=0,width=220)
        Student_PRN_txt=Entry(student_frame,font=("time new roman",15),textvariable=self.sprn)
        Student_PRN_txt.place(x=1250,y=0)

        #Academic Details
        
        Bill_No=Label(form_frame,text="Admission no",font=("times new roman",15))
        Bill_No.place(x=20,y=0,width=130)
        Bill_No_txt=Entry(form_frame,font=("times new roman",15),textvariable=self.billNo)
        Bill_No_txt.place(x=170,y=0,width=200)
        
        Academic_year=Label(form_frame,text="Academic Year",font=("times new roman",15))
        Academic_year.place(x=20,y=50,width=130)
        self.years.insert(0,"select")
        self.Academic_year_List=ttk.Combobox(form_frame,font=("times new roman",15), values=self.years)
        self.Academic_year_List.current(0)
        self.Academic_year_List.place(x=170,y=50,width=200)
        
        Semister=Label(form_frame,text="Semester",font=("times new roman",15))
        Semister.place(x=20,y=100,width=130)
        self.sem.insert(0,"select")
        self.Semister_List=ttk.Combobox(form_frame,font=("times new roman",15), values=self.sem)
        self.Semister_List.current(0)
        self.Semister_List.place(x=170,y=100,width=200)

        Branch_name=Label(form_frame,text="Branch",font=("times new roman",15))
        Branch_name.place(x=20,y=150,width=130)
        self.branch.insert(0,"select")
        self.Branch_name_List=ttk.Combobox(form_frame,font=("times new roman",15),values=self.branch)
        self.Branch_name_List.current(0)
        self.Branch_name_List.place(x=170,y=150,width=200)

        Fee_amount_lbl=Label(form_frame,text="Amount",font=("times new roman",15))
        Fee_amount_lbl.place(x=20,y=250,width=130)
        Fee_amount_txt=Entry(form_frame,font=("times new roman",15),textvariable=self.amount)
        Fee_amount_txt.place(x=170,y=250,width=200)
        
        
        #billing Area
        scrolly=Scrollbar(table_frame,orient=VERTICAL)
        self.billarea=Text(table_frame,yscrollcommand=scrolly.set,font=("times new roman",15),fg="blue")
        scrolly.pack(side=RIGHT,fill=Y)
        scrolly.config(command=self.billarea.yview)
        self.billarea.pack(fill=BOTH,expand=1)
        
        #buttons
        self.Add_btn=Button(button_frame,text="     Add    ",font=("times new roman",15),command=self.ADD)
        self.Add_btn.place(x=80,y=15)
        self.cal_bill_btn=Button(button_frame,text="Calculate",font=("times new roman",15),command = self.makeBill)
        self.cal_bill_btn.place(x=270,y=15)
        self.save_bill_btn=Button(button_frame,text="   Print   ",font=("times new roman",15),command=self.printBill)
        self.save_bill_btn.place(x=450,y=15)
        self.reset_bill_btn=Button(button_frame,text="    Save    ",font=("times new roman",15),command=self.saveBill)
        self.reset_bill_btn.place(x=80,y=70)
        self.print_btn=Button(button_frame,text="   Reset   ",font=("times new roman",15))
        self.print_btn.place(x=270,y=70)
        self.exit_btn=Button(button_frame,text="    Exit   ",font=("times new roman",15))
        self.exit_btn.place(x=450,y=70)
        
        self.heading()
        
    def ADD(self):
        if self.billNo.get()==0:
            messagebox.showinfo("info","please enter Admission No")
        elif self.Academic_year_List.get()=="select":
            messagebox.showinfo("info","please select Year")
        elif self.Semister_List.get()=="select":
            messagebox.showinfo("info","please enter semester")
        elif self.Branch_name_List.get()=="select":
            messagebox.showinfo("info","please select branch")
        elif self.amount.get()==0:
            messagebox.showinfo("info","please enter fee amount")
        else:
            a=self.amount.get()
            s=self.Semister_List.get()
            b=self.Branch_name_List.get()
            self.tlist.append(a)
            print(self.tlist)
            self.billarea.insert(END,f'\n\n {b} \t\t {self.Academic_year_List.get()} \t\t {s} \t\t {a} ')
        
    def makeBill(self):
        if len(self.sname.get())==0 and len(self.smob.get())==0 and len(self.sprn.get())==0:
            messagebox.showinfo("info","please enter Student details")
        elif self.billNo.get()==0:
            messagebox.showinfo("info","please enter Admission No")
        elif self.Academic_year_List.get()=="select":
            messagebox.showinfo("info","please select Year")
        elif self.Semister_List.get()==0:
            messagebox.showinfo("info","please enter semester")
        elif self.Branch_name_List.get()=="select":
            messagebox.showinfo("info","please select branch")
        elif self.amount.get()==0:
            messagebox.showinfo("info","please enter fee amount")
        else:
            a=self.amount.get()
            s=self.Semister_List.get()
            b=self.Branch_name_List.get()
            self.tlist.append(a)
            print(self.tlist)
            self.billarea.insert(END,f'\n\n {b} \t\t {self.Academic_year_List.get()} \t\t {s} \t\t {a} ')
            space=""
            total=sum(self.tlist)
            self.billarea.insert(6.16,self.billNo.get()) # type: ignore
            self.billarea.insert(8.16,self.sname.get())
            self.billarea.insert(9.16,self.smob.get())
            self.billarea.insert(10.16,self.sprn.get())
            self.billarea.insert(END,"\n\n*************************************")
            self.billarea.insert(END,f'\n Total={space*30} {total}')
            self.billarea.insert(END,f'\n 9% CGST={space*50} {total*(9/100)}')
            self.billarea.insert(END,f'\n 9% SGST={space*50} {total*(9/100)}')
            self.billarea.insert(END,f'\n Bill={space*50} {total+total*(9/100)+total*(9/100)}')

    def saveBill(self):
        opt=messagebox.askyesno("RECEIPT","Are you want to save receipt ?")
        if opt==True:
            self.bill_data=self.billarea.get(1.0,END)
            #file handling 
            fh=open("receipt/"+self.billNo.get()+".txt",'w') 
            fh.write(self.bill_data)
            fh.close()
            
    def printBill(self):
        q=self.billarea.get(1.0,'end-1c')
        filename=tempfile.mktemp('.txt')
        open(filename,'w').write(q)
        os.startfile(filename,"print")
            
            
    def heading(self):
        self.billarea.delete(1.0,END)
        self.billarea.insert(END," \t\tS  C  H  O  O  L\t\tN  A  M  E")
        self.billarea.insert(END,"\n\n\t\t\t\t RECEIPT ")
        self.billarea.insert(END,"\n\t_________________________________________________________________")
        self.billarea.insert(END,"\n\t")
        self.billarea.insert(END,f'\n Admission no \t\t')
        self.billarea.insert(END,"\n\t")
        self.billarea.insert(END,f'\n Student Name \t\t')
        self.billarea.insert(END,f'\n Mobile no \t\t')
        self.billarea.insert(END,f'\n Student Roll no \t\t')
        self.billarea.insert(END,"\n\n  Branch  \t\t  Year  \t\t  Semester  \t\t Total Fee")



if __name__=='__main__':
    win=Tk()
    app=College(win)
    win.mainloop()
