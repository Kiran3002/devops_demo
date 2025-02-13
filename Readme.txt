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

from django.db import models

# Create your models here.
class Student(models.Model):
    name=models.TextField(max_length=200,blank=False)
    email=models.EmailField(max_length=200)
    register_number=models.BigIntegerField(blank=False)
    course=models.CharField(max_length=30,blank=False)
    batch=models.BigIntegerField()
    Department=models.CharField(max_length=50,blank=False)
    is_active=models.BooleanField(default=True)
    Created_at=models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
    

class Achievement(models.Model):
    student=models.ForeignKey(Student,on_delete=models.CASCADE)
    Title=models.CharField(max_length=50,blank=False)
    Description=models.TextField(max_length=200,blank=True)
    Proof=models.ImageField(upload_to='proof_img',null=True,blank=True)
    Category=models.CharField(max_length=50)
    Date=models.DateField(null=True,blank=True)
    created_at_ach=models.DateTimeField(auto_now_add=True)
    STATUS_PENDING = 'P'
    STATUS_COMPLETED = 'A'
    STATUS_FAILED = 'R'
    STATUS_CHOICES = [
        (STATUS_PENDING, 'Pending'),
        (STATUS_COMPLETED, 'Approved'),
        (STATUS_FAILED, 'Rejected'),
    ]

    status = models.CharField(max_length=1, choices=STATUS_CHOICES, default=STATUS_PENDING)

    def __str__(self):
        return self.Title
    


class category(models.Model):
    cat_name=models.ForeignKey(Achievement,on_delete=models.CASCADE)
    description_cat=models.TextField(max_length=200)
    created_at_cat=models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.cat_name


{% extends 'base.html' %}

{% block content %}
<div class="container">
    <form class="col-6 mx-auto card p-3 shadow-lg" method="POST" >
        {% csrf_token %}
        <br>
        <h3>Add student</h3>
        <hr>
        <div class="col-12">
          <label for="exampleInputEmail1">Student Name</label>
          <input name="name" id="name" type="text" class="form-control" >
         </div>
        <div class="col-12">
          <label for="exampleInputPassword1"  >Email</label>
          <input name="email" id="email" type="email" class="form-control" >
        </div>
        <div class="col-12">
            <label for="exampleInputPassword1"  >Register Number</label>
            <input name="register_number" id="register_number" type="number" class="form-control" >
        </div>
        <div class="col-12">
            <label for="exampleInputPassword1"  >Course</label>
            <input name="course" id="course" type="text" class="form-control" >
        </div>
        <div class="col-12">
            <label for="exampleInputPassword1"  >Batch</label>
            <input name="batch" id="batch" type="number" class="form-control" >
        </div>
        <div class="col-12">
            <label for="exampleInputPassword1"  >Department</label>
            <input name="Department" id="Department" type="text" class="form-control" >
        </div>
        <button type="submit" class="btn btn-success">Add student</button>
      </form>
</div>
{% endblock content %}


{% extends 'base.html' %}
{% block content %}
    <div class="container">
        {% if student %}
        {% for i in student %}
        <div class="col-12 border mb-3">
            <div class="fs-5 fw-bold">name: {{i.name}}</div>
            <div class="fs-5 fw-bold">email: {{i.email}}</div>
            <div class="fs-5 fw-bold">register number: {{i.register_number}}</div>
            <div class="fs-5 fw-bold">course: {{i.course}}</div>
            <div class="fs-5 fw-bold">batch: {{i.batch}}</div>
            <div class="fs-5 fw-bold">department: {{i.Department}}</div>
            <button type="button" class="btn btn-warning">Delete</button>
            <button type="button" class="btn btn-info">Edit</button>
            <button type="button" class="btn btn-light">add achievement</button>
            <button type="button" class="btn btn-dark">Dark</button>
              
        </div>
        {% endfor %}
        {% endif %}
    </div>
{% endblock content %}
