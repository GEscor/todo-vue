<template>
   <div class="filter-field">
      <h2>Фильтр</h2>
      <label class="filter all"><input
        type="radio"
        name="filter"
        value="all"
        :checked="filter === 'all'"
        @change="$emit('filter-select', $event.target.value)"
      />Все</label>
      <label class="filter low"><input
        type="radio"
        name="filter"
        value="low"
        :checked="filter === 'low'"
        @change="$emit('filter-select', $event.target.value)"
      />Обычные
      </label>
      <label class="filter med"><input
        type="radio"
        name="filter"
        value="med"
        :checked="filter === 'med'"
        @change="$emit('filter-select', $event.target.value)"
      />Важные
      </label>
      <label class="filter high"><input
        type="radio"
        name="filter"
        value="high"
        :checked="filter === 'high'"
        @change="$emit('filter-select', $event.target.value)"
      />Очень важные
      </label>
    </div>
</template>

<script>
export default {
    name: 'TaskFilter',
    props: {
        filter: String
    }
}
</script>

<style>
.filter-field {
    text-align: center;
    border-bottom: 5px solid #fff;
}
.filter{
    display:inline-block;
    padding: 10px 5px;
    margin: 5px;
    width: 23%;
    min-width: 200px;
    border-radius: 10px;
}
.low {
    background-color:#C0E5A5;
}
.med {
    background-color: #EFEF83;
}
.high {
    background-color: #E57472;
}
.all {
    background-color: #fff;
}

</style>