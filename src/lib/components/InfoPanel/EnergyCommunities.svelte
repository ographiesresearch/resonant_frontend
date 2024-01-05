<script>
    import Tabs from '$lib/components/InfoPanel/Tabs.svelte';
    import CriterionCard from '$lib/components/InfoPanel/CriterionCard.svelte';
    export let data;
    
    let mine_text;

    $: if (data?.mine && data?.gen) {
        mine_text = "A coal mine has closed and a coal-fired electric generator has been retired nearby after 1999."
    } else if (data?.mine) {
        mine_text = 'A coal mine was closed nearby after 1999.'
    } else if (data?.gen) { 
        mine_text = 'A coal-fired electric generator was retired nearby after 1999.'
    } else {
        mine_text = 'No coal mines have closed, nor electric generating units retired nearby after 1999.'
    }

    let adj_text;
    $: if (data?.mine) {
        adj_text = `A coal mine has closed or a coal-fired electric generator has been retired in an adjoining tract since 1999.`
    } else {
        adj_text = `No coal mines have closed and no electric generating units have retired in adjoining census tracts since 1999.`
    }

    let ec_text;
    $: if (data?.ec) {
        ec_text = `This site is in an area with <code>0.17%</code> or greater direct employment or <code>25%</code> or greater local tax revenues related to the extraction, processing, transport, or storage of coal, oil, or natural gas and an employment rate at or above the national average unemployment rate for the previous year.`
    } else {
        ec_text = 'This site does not meet fossil fuel employment or general unemployment criteria.'
    }
    
    $: status = data?.nrg_comm;

    $: items = [
            {
                desc: "Mine or Generator Closure",
                status: (data?.mine || data?.gen),
                text: mine_text,
                citation: 'U.S. Department of Energy, National Energy Technology Laboratory, Interagency Working Group on Coal & Power Plant Communities & Economic Revitalization, U.S. Department of the Treasury. 2023. <a href="https://edx.netl.doe.gov/dataset/ira-energy-community-data-layers"><em>IRA Energy Community Data Layers</em></a>. April 3.'
            },
            {
                desc: "Adjoining Closure",
                status: data?.adj,
                text: adj_text,
                citation: 'U.S. Department of Energy, National Energy Technology Laboratory, Interagency Working Group on Coal & Power Plant Communities & Economic Revitalization, U.S. Department of the Treasury. 2023. <a href="https://edx.netl.doe.gov/dataset/ira-energy-community-data-layers"><em>IRA Energy Community Data Layers</em></a>. April 3.'
            },
            {
                desc: "Employment",
                status: data?.ec,
                text: ec_text,
                citation: 'U.S. Department of Energy, National Energy Technology Laboratory, Interagency Working Group on Coal & Power Plant Communities & Economic Revitalization, U.S. Department of the Treasury. 2023. <a href="https://edx.netl.doe.gov/dataset/ira-energy-community-data-layers"><em>IRA Energy Community Data Layers</em></a>. April 3.'
            },
        ]
</script>
<CriterionCard {status} title="Energy Communities">
    <Tabs {items}/>
</CriterionCard>
