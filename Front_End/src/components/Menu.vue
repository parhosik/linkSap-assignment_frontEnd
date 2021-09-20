<template>
  <sidebar-menu :menu="menu" />
</template>

<script>
    export default {
      name : 'globalmenu',
        data() {
            return {
                menu: [
                    {
                        header: true,
                        title: 'Main Navigation',
                       
                    },
                    {
                        href: '/home',
                        title: 'Recipes',
                        icon: 'fa fa-user'
                    },
                    {
                        href: '/addRecipe',
                        title: 'Add New Recipe',
                        icon: 'fa fa-user',
                  
                    },
                    {
                        href: '/login',
                        title: 'Logout',
                        icon: 'fa fa-user',
              
                    }
                ]
            }
        }
    }
</script>