# Main Menu Component

```jsx
function MainMenuComponent() {
    const tabs = [
        { 
            id: 'tab1', 
            label: 'Dashboard',
            componentPath: 'system/datacore/mainMenu/tab1.component.md',
            headerName: 'Tab 1 Component'
        },
        { 
            id: 'tab2', 
            label: 'Notes & Queries',
            componentPath: 'system/datacore/mainMenu/tab2.component.md',
            headerName: 'Tab 2 Component'
        },
        { 
            id: 'tab3', 
            label: 'Analytics',
            componentPath: 'system/datacore/mainMenu/tab3.component.md',
            headerName: 'Tab 3 Component'
        }
    ];
    
    // Function to dynamically load and render tab content
    const renderTabContent = async (tabId) => {
        const tab = tabs.find(t => t.id === tabId);
        if (!tab) return <div>Tab not found</div>;
        
        try {
            // Use DataCore's require function to load the component
            const { [tab.headerName.replace(/\s+/g, '')]: TabComponent } = await dc.require(
                dc.headerLink(tab.componentPath, tab.headerName)
            );
            return <TabComponent />;
        } catch (error) {
            return <div>Error loading tab: {error.message}</div>;
        }
    };
    
    // Simple click handler for tab switching
    const handleTabClick = async (tabId) => {
        // Update active tab styling
        document.querySelectorAll('.tab-button').forEach(btn => {
            btn.classList.remove('active');
        });
        document.querySelector(`[data-tab="${tabId}"]`).classList.add('active');
        
        // Load and render the new tab content
        const contentContainer = document.querySelector('.tab-content-container');
        if (contentContainer) {
            contentContainer.innerHTML = '<div class="loading">Loading...</div>';
            
            try {
                const tab = tabs.find(t => t.id === tabId);
                const { [tab.headerName.replace(/\s+/g, '')]: TabComponent } = await dc.require(
                    dc.headerLink(tab.componentPath, tab.headerName)
                );
                
                // Create a temporary container to render the component
                const tempDiv = document.createElement('div');
                const root = ReactDOM.createRoot ? ReactDOM.createRoot(tempDiv) : ReactDOM.render;
                
                if (ReactDOM.createRoot) {
                    root.render(<TabComponent />);
                } else {
                    ReactDOM.render(<TabComponent />, tempDiv);
                }
                
                // Replace the loading content with the rendered component
                setTimeout(() => {
                    contentContainer.innerHTML = tempDiv.innerHTML;
                }, 100);
                
            } catch (error) {
                contentContainer.innerHTML = `<div class="error">Error loading tab: ${error.message}</div>`;
            }
        }
    };
    
    // Default tab content component (for initial render)
    const DefaultTabContent = () => {
        return (
            <div className="tab-content">
                <h3>Dashboard Overview</h3>
                <div className="content-section">
                    <p>Welcome to your personal dashboard! This content is loaded from tab1.component.md</p>
                    <div className="loading">Loading dynamic content...</div>
                </div>
            </div>
        );
    };
    
    // Load the first tab content on component mount
    React.useEffect(() => {
        handleTabClick('tab1');
    }, []);
    
    return (
        <div className="main-menu-container">
            <div className="tab-navigation">
                {tabs.map(tab => (
                    <button
                        key={tab.id}
                        className={`tab-button ${tab.id === 'tab1' ? 'active' : ''}`}
                        data-tab={tab.id}
                        onClick={() => handleTabClick(tab.id)}
                    >
                        {tab.label}
                    </button>
                ))}
            </div>
            <div className="tab-content-container">
                <DefaultTabContent />
            </div>
            <style>{`
                .main-menu-container {
                    width: 100%;
                    max-width: 800px;
                    margin: 0 auto;
                    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
                }
                
                .tab-navigation {
                    display: flex;
                    border-bottom: 2px solid #e1e5e9;
                    margin-bottom: 20px;
                    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                    border-radius: 8px 8px 0 0;
                    padding: 0;
                    overflow: hidden;
                }
                
                .tab-button {
                    flex: 1;
                    padding: 12px 20px;
                    border: none;
                    background: transparent;
                    color: rgba(255, 255, 255, 0.8);
                    cursor: pointer;
                    font-size: 14px;
                    font-weight: 500;
                    transition: all 0.3s ease;
                    position: relative;
                }
                
                .tab-button:hover {
                    background: rgba(255, 255, 255, 0.1);
                    color: white;
                }
                
                .tab-button.active {
                    background: rgba(255, 255, 255, 0.2);
                    color: white;
                    font-weight: 600;
                }
                
                .tab-button.active::after {
                    content: '';
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    right: 0;
                    height: 3px;
                    background: white;
                }
                
                .tab-content-container {
                    background: white;
                    border-radius: 0 0 8px 8px;
                    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
                    min-height: 400px;
                    padding: 24px;
                }
                
                .tab-content {
                    animation: fadeIn 0.3s ease-in;
                }
                
                @keyframes fadeIn {
                    from { opacity: 0; transform: translateY(10px); }
                    to { opacity: 1; transform: translateY(0); }
                }
                
                .tab-content h3 {
                    margin: 0 0 16px 0;
                    color: #2c3e50;
                    font-size: 24px;
                    font-weight: 600;
                }
                
                .content-section {
                    color: #555;
                    line-height: 1.6;
                }
                
                .loading {
                    background: #f8f9fa;
                    border: 1px solid #e9ecef;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #667eea;
                    color: #6c757d;
                    font-style: italic;
                }
                
                .error {
                    background: #f8d7da;
                    border: 1px solid #f5c6cb;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #dc3545;
                    color: #721c24;
                }
            `}</style>
        </div>
    );
}

return { MainMenuComponent };
```