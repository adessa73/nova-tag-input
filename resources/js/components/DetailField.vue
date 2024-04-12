<template>
    <panel-item :index="index" :field="fieldLabel" />
  </template>
  
  <script>
  let vm;
  
  export default {
    data() {
      return {
        tags: '',
        fieldLabel: {},
        tagsWrapperClass: 'nti-tags-wrapper',
        tagClass: 'nti-tag',
      };
    },
    props: ['index', 'resource', 'resourceName', 'resourceId', 'field'],
    mounted() {
      vm = this;
      this.fieldLabel = { ...this.field };
      let items = this.field.value;
      if (typeof items === 'string') {
        // Se os itens são uma string, vamos tentar analisá-los como JSON
        try {
          items = JSON.parse(items);
        } catch (e) {
          // Em caso de erro na análise JSON, trate como uma string simples
          items = [items];
        }
      }
      if (items instanceof Array && items.length) {
        items.forEach(function (item, index) {
          // Backward compatibility in case tags are stored as object
          let label = (typeof item === 'object' && item.hasOwnProperty('text')) ? item.text : item;
          vm.tags += '<span class="' + vm.tagClass + '">' + label + '</span>';
        });
        this.fieldLabel.value = '<div class="' + this.tagsWrapperClass + '">' + vm.tags + '</div>';
        // Set the label as HTML
        this.fieldLabel.asHtml = true;
      } else {
        this.fieldLabel.value = '—';
      }
    },
  };
  </script>
