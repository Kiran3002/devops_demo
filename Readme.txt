from django.shortcuts import render,redirect
from .models import Student,Achievement,category
# Create your views here.
def index(request):
    all_students=Student.objects.filter(is_active=True)
    context={
        'student':all_students
    }
    return render(request,'student/index.html',context)


def add_student(request):
    if request.method=="POST":
        name=request.POST['name']
        email=request.POST['email']
        register_number=request.POST['register_number']
        course=request.POST['course']
        batch=request.POST['batch']
        Department=request.POST['Department']
       
        students=Student(name=name,email=email,register_number=register_number,course=course,batch=batch,Department=Department)
        students.save()
        return redirect('index')
    else:
        return render(request,'student/add_student.html')
    
def add_achievement(request):
    if request.method=="POST":
        Title=request.POST['Title']
        Description=request.POST['Description']
        Proof=request.IMAGE['Proof']
        Category=request.POST['Category']
        Date=request.POST['Date']
        achievement=Achievement(Title=Title,Description=Description,Proof=Proof,Category=Category,Date=Date)
        achievement.save()
        return redirect('index')
    else:
        return render(request,'student/add_achievement.html')
