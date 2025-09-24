# Tab 1 Component

```jsx
function Tab1Component() {
    return (
        <div className="tab-content">
            <h3>Dashboard Overview</h3>
            <div className="content-section">
                <p>Welcome to your personal dashboard! This is the main overview tab where you can see:</p>
                <ul>
                    <li>Recent notes and updates</li>
                    <li>Quick statistics</li>
                    <li>Important reminders</li>
                </ul>
                <div className="placeholder-widget">
                    <h4>Recent Activity</h4>
                    <p>Your latest notes and modifications will appear here.</p>
                </div>
            </div>
        </div>
    );
}

return { Tab1Component };
```