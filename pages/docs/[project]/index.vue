<template>
    <NuxtLayout v-if="project && project.documentation" name="docs">
        <Head>
            <Title>{{ project.name }} Docs</Title>
            <Meta name="og:title" :content="`${project.name} Docs &mdash; William278.net`" />
            <Meta name="twitter:title" :content="`${project.name} Docs &mdash; William278.net`" />
            <Meta name="description" :content="description" />
            <Meta name="og:description" :content="description" />
            <Meta name="twitter:description" :content="description" />
            <Meta v-if="project.icon && project.icon.png" name="og:image" :content="`/images/icons/${project.icon.png}`" />
            <Meta v-if="project.icon && project.icon.png" name="twitter:image" :content="`/images/icons/${project.icon.png}`" />
        </Head>
        <Overbar>
            <Breadcrumbs :crumbs="[{ name: 'Home', link: '/' }, { name: 'Docs', link: '/docs' }]" />
            <ButtonLink hollow icon="fa6-solid:pencil" :link="`${project.repository}/tree/master/docs`">Edit</ButtonLink>
        </Overbar>
        <article>
            <ContentDoc :head="false" :path="'/docs/project/' + project.id.toLowerCase() + '/home'"  />
        </article>
        <template #sidebar>
            <h1>
                <IconifiedProject centered :project="project" />
            </h1>
            <ContentDoc :head="false" :path="'/docs/project/' + project.id.toLowerCase() + '/_sidebar'" />
        </template>
    </NuxtLayout>
    <NuxtLayout v-else name="default">
        <ErrorPage>
            Docs not found:&nbsp;
            <PathLine>
                <NuxtLink to="/">Home</NuxtLink>
                <BreadcrumbDivider />
                <NuxtLink to="/docs/">Docs</NuxtLink>
                <BreadcrumbDivider />
                <InvalidPage v-if="project" :name="project.name" />
                <InvalidPage v-else :name="$route.params.project" />
            </PathLine>
        </ErrorPage>
    </NuxtLayout>
</template>

<script setup>
import InvalidPage from '../../../components/InvalidPage.vue';
import PathLine from '../../../components/content/PathLine.vue';
import projects from '/assets/data/projects.json'

const { params } = useRoute()
const project = computed(() => projects.find(project => project.id.toLowerCase() === params.project.toLowerCase()))
const description = computed(() => project.value ? `Welcome to the documentation for ${project.value.name} &mdash; ${project.value.tagline} &mdash; available on William278.net` : 'William278.net')
</script>