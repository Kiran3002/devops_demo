{% extends 'base.html' %}
{% block content %}
    <div class="container">
        {% if student %}
        {% for i in student %}
        <div class="col-12 border mb-3">
            <table class="table table-striped table-bordered table-sm">  
                <thead class="thead-dark">  
                <tr>  
                    <th>Student Name</th>  
                    <th>Student Email</th>  
                    <th>Register number</th>  
                    <th>course</th>  
                    <th>Batch</th>  
                    <th>Department</th>
                </tr>  
                </thead>  
                <tbody>  
                <tr>  
                    <td>{{ i.name }}</td>  
                    <td>{{ i.email }}</td>  
                    <td>{{ i.register_number }}</td>  
                    <td>{{ i.course }}</td>  
                    <td>{{i.batch}}</td>
                    <td>{{i.department}}</td>
                    <td>  
                        <button type="button" class="btn btn-warning">Delete</button>  
                    </td>  
                    <td>
                        <button type="button" class="btn btn-info">Edit</button>
                    </td>
                    <td>
                        <button type="button" class="btn btn-light">add achievement</button>
                    </td>
                </tr>  
                </tbody>  
            </table>
            
        </div>
        {% endfor %}
        {% endif %}
    </div>
{% endblock content %}
