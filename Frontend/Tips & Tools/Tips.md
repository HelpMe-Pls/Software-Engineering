[[Tips & Tools/Meta]]
[[Interview prep]]
[[Best practices]]
[[code-review.png |Code Review]]

- Check for empty object: 
```jsx
// Render this component only when `detailCampaign` object has data:
{Object.keys(detailCampaign).length > 0 && (
        <CardLayout
          title={<FormattedMessage id="pages.campaign-management.campaign.info" />}
          icon={<InfoCircleFilled style={{ color: '#526eff' }} />}
        >
          <CampaignInfo detailCampaign={detailCampaign} phoneCall={phoneCall} />
        </CardLayout>
)}
```

- Render a number of skeleton components based on a prop `count`:
```jsx
<div className="grid grid-cols-1 gap-6 lg:grid-cols-3">
    {Array.from({ length: category.count }).map((_, i) => (
          <SkeletonCard key={i} />
     ))}
</div>
```
- Centering trick with 1 side having content [in flex layout](https://github.com/css-for-js/sole-and-ankle/commit/ad2863febfccdcc58d87e5ad5210b2ebfcd2faf8) by using  *`flex: 1`  elements* to squeeze your desired center content.
- Generalized FizzBuzz using Map:
```js
var multipliers = [
  [3, "Fizz"],
  [5, "Buzz"]
]

for (let i = 1; i <= 100; i++) {
  let output = "";

  multipliers.forEach(function(item) {
     if (i % item[0] === 0) {
       output += item[1];
     }
  }); 

  console.log(output || i);
}
```

- If your component has styling transformation, better isolate that animation part to an API and keep your main component SoC from that animation:
```tsx
//==========>> <Drawer/> helpers <<==========
const openedMixin = (theme: Theme): CSSObject => ({
	width: drawerWidth,
	overflowX: 'hidden',
	transition: theme.transitions.create('width', {
		easing: theme.transitions.easing.sharp,
		duration: theme.transitions.duration.enteringScreen,
	}),
});
const closedMixin = (theme: Theme): CSSObject => ({
	width: `calc(${theme.spacing(7)} + 1px)`,
	overflowX: 'hidden',
	transition: theme.transitions.create('width', {
		easing: theme.transitions.easing.sharp,
		duration: theme.transitions.duration.leavingScreen,
	}),
});

const Drawer = styled(MuiDrawer, { shouldForwardProp: (prop) => prop !== 'open' })(
	({ theme, open }) => ({
		width: drawerWidth,
		flexShrink: 0,
		whiteSpace: 'nowrap',
		boxSizing: 'border-box',
		...(open && {
			...openedMixin(theme),
			'& .MuiDrawer-paper': openedMixin(theme),
		}),
		...(!open && {
			...closedMixin(theme),
			'& .MuiDrawer-paper': closedMixin(theme),
		}),
	}),
);

//=================|| Main Component ||===================
export default function CustomDrawer() {
	const [open, setOpen] = useState(false);
	const toggleDrawer = () => {
		setOpen(!open)
	}
	return (
			<Drawer variant="permanent" open={open}>
				{/*Do some*/}
			</Drawer>
	);
}

```