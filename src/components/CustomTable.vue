<template>
    <div>
         <table>
            <tr>
                <th>Nombre</th>
                <th>Edad</th>
                <th>Salario</th>
            </tr>
            <tr v-for="(row,key) in content">
                <td>{{row.employee_name}}</td>
                <td>{{row.employee_age}}</td>
                <td>{{row.employee_salary}}</td>
            </tr>
        </table> 
    </div>
</template>

<script>
export default{
    props: {
        content: Array,
    },
}
</script>